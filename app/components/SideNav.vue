<template>
    <UCard v-if="activeSection !=='hero'" class="w-fit fixed  left-0 z-50 top-40 bottom-40 ring-0  bg-transparent flex flex-col items-center justify-center p-0 md:p-0">
            <!-- a column of invisible buttons which show their label on hover and have a circle icon that turns solid if a section is active -->
        <div class="flex my-auto flex-col gap-2">
            <UButton
                v-for="section in navigationSections"
                :key="section.id"
                :ui="{label:'hover:visible '}"
                variant="ghost"
                :icon="activeSection === section.id ? 'i-material-symbols-circle' : 'i-material-symbols-circle-outline'"
                color="secondary"
                :label="activeSection === section.id ? section.label : ''"
                class="w-full text-left flex items-center gap-2"
                :active="activeSection === section.id"
                square
                @click="scrollToSection(section.id, true)"
            />
            </div>
        </UCard>
</template>
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