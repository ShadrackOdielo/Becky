
<template>
	<nav
ref="navbarRef" class="fixed top-0 left-0 right-0 z-50 transition-all duration-300" :class="[
		isScrolled
			? 'bg-white/80 dark:bg-neutral-900/80 backdrop-blur-md border-b border-neutral-200/20 dark:border-neutral-700/20 shadow-lg'
			: 'bg-transparent'
	]" role="navigation" aria-label="Main navigation">
		<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
			<div class="flex items-center justify-between h-16 lg:h-20">

				<!-- Logo/Brand -->
				<div class="flex-shrink-0">
					<UButton 
						to="/" 
						:class="isScrolled ? 'text-secondary-600 dark:text-secondary-400' : 'text-white dark:text-white'"
variant="ghost" size="lg" class="font-bold text-xl bg-gradient-to-r from-secondary-500 via-accent-500 to-secondary-600 bg-clip-text text-transparent hover:scale-105 transition-transform duration-200"
						aria-label="Go to top of page"
						
						@click="scrollToSection('hero')">
						Rebecca Auma

					</UButton>
				</div>

				<!-- Desktop Navigation -->

    	<div  class="hidden lg:block">
					<div class="flex items-center space-x-1">
						<UButton
						v-for="section in navigationSections" :key="section.id" variant="ghost"
							size="sm" class="relative px-4 py-2 rounded-lg transition-all duration-200 hover:bg-secondary-50 dark:hover:bg-secondary-900/20"
							:class="[
								activeSection === section.id
									? 'text-secondary-600 dark:text-secondary-400 bg-secondary-50 dark:bg-secondary-900/20'
									: 'text-neutral-700 dark:text-neutral-300 hover:text-secondary-600 dark:hover:text-secondary-400'
							]"
							:aria-label="`Navigate to ${section.label} section`" :aria-current="activeSection === section.id ? 'page' : undefined"
							@click="scrollToSection(section.id)">
							{{ section.label }}

							<!-- Active indicator -->
							<UIcon
:name="activeSection === section.id ? 'mdi:circle' : ''"
								class="absolute bottom-0 left-1/2 transform -translate-x-1/2 w-1 h-1 bg-secondary-500 rounded-full"
								:class="activeSection === section.id ? 'block' : 'hidden'"
								:aria-label="`Active section indicator for ${section.label}`"
								:aria-current="activeSection === section.id ? 'true' : 'false'"/>
						</UButton>
						
						

					</div>
					
				</div>
				

				<!-- Theme Toggle & Mobile Menu Button -->
				<div class="flex items-center space-x-2">
					<!-- Theme Toggle -->
					<ColorModeButton />
					<ColorToggle />
					<!-- Mobile Menu Button -->
					<UButton
variant="ghost" size="md" class="lg:hidden p-2" :aria-label="isMobileMenuOpen ? 'Close navigation menu' : 'Open navigation menu'"
						:aria-expanded="isMobileMenuOpen"
						aria-controls="mobile-menu" @click="toggleMobileMenu">
						<UIcon
:name="isMobileMenuOpen ? 'mdi:close' : 'mdi:menu'"
							class="w-6 h-6 text-neutral-700 dark:text-neutral-300" />
					</UButton>
				</div>
			</div>
		</div>

		<!-- Mobile Navigation Menu -->
		<Transition
enter-active-class="transition duration-200 ease-out" enter-from-class="opacity-0 scale-95"
			enter-to-class="opacity-100 scale-100" leave-active-class="transition duration-150 ease-in"
			leave-from-class="opacity-100 scale-100" leave-to-class="opacity-0 scale-95">
			<div
v-if="isMobileMenuOpen" id="mobile-menu"
				class="lg:hidden absolute top-full left-0 right-0 bg-white/95 dark:bg-neutral-900/95 backdrop-blur-md border-b border-neutral-200/20 dark:border-neutral-700/20 shadow-lg"
				role="menu" aria-orientation="vertical">
				<div class="px-4 py-3 space-y-1">

					<UButton

v-for="section in navigationSections" :key="`mobile-${section.id}`"
						color="secondary"
						variant="ghost" size="md" class="w-full justify-start px-4 py-3 rounded-lg transition-all duration-200"
						:class="[
							activeSection === section.id
								? 'text-secondary-600 dark:text-secondary-400 bg-secondary-50 dark:bg-secondary-900/20'
								: 'text-neutral-700 dark:text-neutral-300 hover:text-secondary-600 dark:hover:text-secondary-400 hover:bg-secondary-50 dark:hover:bg-secondary-900/20'
						]" :aria-label="`Navigate to ${section.label} section`" :aria-current="activeSection === section.id ? 'page' : undefined"
						role="menuitem" @click="scrollToSection(section.id, true)">
						{{ section.label }}
					</UButton>

				</div>
			</div>
		</Transition>
	</nav>
</template>

<!-- Script Section -->
<script setup lang="ts">
// ------------ IMPORTS
import { ref, onMounted, onBeforeUnmount } from 'vue'

// ------------ TYPES
interface NavigationSection {
	id: string
	label: string
}

const route = useRoute()

// ------------ REACTIVE STATE
const navbarRef = ref<HTMLElement | null>(null)
const isScrolled = ref(false)
const isMobileMenuOpen = ref(false)
const activeSection = ref('hero')

// ------------ NAVIGATION SECTIONS
const navigationSections: NavigationSection[] = [
	{ id: 'hero', label: 'Home' },
	{ id: 'about', label: 'About' },
	{ id: 'services', label: 'Services' },
	{ id: 'education', label: 'Education' },
	{ id: 'experience', label: 'Experience' },
	{ id: 'contact', label: 'Contact' }

]

// ------------ SCROLL HANDLING
let scrollTimeout: NodeJS.Timeout | null = null

function handleScroll() {
	// Throttle scroll events for performance
	if (scrollTimeout) return

	scrollTimeout = setTimeout(() => {
		// Update navbar background based on scroll position
		isScrolled.value = window.scrollY > 20

		// Update active section based on scroll position
		updateActiveSection()

		scrollTimeout = null
	}, 10)
}

// ------------ ACTIVE SECTION TRACKING
function updateActiveSection() {
	const sections = navigationSections.map(section => {
		const element = document.getElementById(section.id)
		if (!element) return null

		const rect = element.getBoundingClientRect()
		const offset = 100 // Offset for navbar height

		return {
			id: section.id,
			top: rect.top,
			bottom: rect.bottom,
			inView: rect.top <= offset && rect.bottom > offset
		}
	}).filter(Boolean)

	// Find the section that's currently in view
	const currentSection = sections.find(section => section?.inView)

	if (currentSection) {
		activeSection.value = currentSection.id
	} else {
		// If no section is in view, find the closest one
		const closestSection = sections.reduce((closest, section) => {
			if (!section || !closest) return section || closest

			const sectionDistance = Math.abs(section.top)
			const closestDistance = Math.abs(closest.top)

			return sectionDistance < closestDistance ? section : closest
		}, null)

		if (closestSection) {
			activeSection.value = closestSection.id
		}
	}
}

// ------------ NAVIGATION METHODS
function scrollToSection(sectionId: string, closeMobileMenu = false) {
	const element = document.getElementById(sectionId)

	if (element) {
		const navbarHeight = 80 // Approximate navbar height
		const elementPosition = element.offsetTop - navbarHeight

		window.scrollTo({
			top: elementPosition,
			behavior: 'smooth'
		})

		// Update active section immediately for better UX
		activeSection.value = sectionId

		// Close mobile menu if requested
		if (closeMobileMenu) {
			isMobileMenuOpen.value = false
		}

		// Announce navigation for screen readers
		const section = navigationSections.find(s => s.id === sectionId)
		if (section) {
			// Create a temporary announcement element
			const announcement = document.createElement('div')
			announcement.setAttribute('aria-live', 'polite')
			announcement.setAttribute('aria-atomic', 'true')
			announcement.className = 'sr-only'
			announcement.textContent = `Navigated to ${section.label} section`
			document.body.appendChild(announcement)

			// Remove after announcement
			setTimeout(() => {
				document.body.removeChild(announcement)
			}, 1000)
		}
	}
}

function toggleMobileMenu() {
	isMobileMenuOpen.value = !isMobileMenuOpen.value
}

// ------------ KEYBOARD NAVIGATION
function handleKeydown(event: KeyboardEvent) {
	// Close mobile menu on Escape
	if (event.key === 'Escape' && isMobileMenuOpen.value) {
		isMobileMenuOpen.value = false
	}
}

// ------------ LIFECYCLE
let fadeInCleanup: (() => void) | null = null

onMounted(() => {
	// Add scroll listener
	window.addEventListener('scroll', handleScroll, { passive: true })

	// Add keyboard listener
	document.addEventListener('keydown', handleKeydown)

	// Initialize scroll state
	handleScroll()

	// Initialize fade-in animation
	const fadeInElement = navbarRef.value
	if (fadeInElement) {
		fadeInElement.classList.add('opacity-0')
		fadeInElement.classList.remove('hidden')

		fadeInCleanup = () => {
			fadeInElement.classList.remove('opacity-0')
			fadeInElement.classList.add('opacity-100')
		}

		setTimeout(fadeInCleanup, 300) // Delay to allow initial styles to apply
	}
})

onBeforeUnmount(() => {
	// Clean up event listeners
	window.removeEventListener('scroll', handleScroll)
	document.removeEventListener('keydown', handleKeydown)

	// Clean up scroll timeout
	if (scrollTimeout) {
		clearTimeout(scrollTimeout)
	}

	// Clean up fade-in animation
	if (fadeInCleanup) {
		fadeInCleanup()
	}
})
watch(route, to => {
  if (to.path === '/blog') {
    activeSection.value = 'blog'
  } else if (to.path === '/') {
    activeSection.value = 'hero'
    handleScroll()
  }
})
</script>