<template>
  <div class="json-lab">
    <h1>🗄️ JSON Data & Vue Directives Lab</h1>

    <section class="lab-section">
      <h2>📚 Working with JSON Arrays</h2>
      <p>Our <code>authors.json</code> contains an array of author objects.</p>

      <h3>Activity 6: Iterating through Arrays</h3>
      <p>All authors:</p>
      <ul>
        <li v-for="author in authors" :key="author.id">
          {{ author.name }} ({{ author.birthYear }})
        </li>
      </ul>

      <h3>Activity 7: Filtering Arrays</h3>
      <p>Authors born after 1850:</p>
      <ul>
        <li v-for="author in modernAuthors" :key="author.id">
          {{ author.name }} ({{ author.birthYear }})
        </li>
      </ul>

      <h3>Activity 8: Mapping Arrays</h3>
      <p>Famous works:</p>
      <ul>
        <li v-for="work in allFamousWorks" :key="work">
          {{ work }}
        </li>
      </ul>

      <h3>Activity 4: Finding in Arrays</h3>
      <p v-if="orwell">Finding by property: {{ orwell.name }}</p>
      <p v-else>George Orwell was not found.</p>

      <h3>Activity 5 and Activity 9: Nested Arrays/Objects</h3>
      <p v-if="austen">{{ austen.name }}'s works:</p>
      <p v-else>Author with ID 1 was not found.</p>

      <ul v-if="austen && austen.famousWorks">
        <li
          v-for="(work, index) in austen.famousWorks"
          :key="`austen-work-${index}`"
        >
          {{ getWorkTitle(work) }}
        </li>
      </ul>
    </section>

    <section class="lab-section">
      <h2>🏢 Working with JSON Objects</h2>
      <p>Our <code>bookstores.json</code> is a JSON object.</p>

      <h3>Activity 9a: Accessing Properties</h3>
      <p>
        Company:
        {{ companyName }}
      </p>

      <p>
        Total Stores:
        {{ totalStores }}
      </p>

      <h3>Activity 10: Iterating Object Properties</h3>
      <p>Store Types:</p>
      <ul>
        <li
          v-for="(storeType, index) in storeTypes"
          :key="`store-type-${index}`"
        >
          {{ getStoreTypeName(storeType) }} -
          {{ getStoreTypeCount(storeType) }} stores
        </li>
      </ul>

      <h3>Activity 11: Nested Objects</h3>
      <p>Opening Hours:</p>
      <ul>
        <li
          v-for="(hours, day) in openingHours"
          :key="day"
        >
          {{ day }}:
          {{ getOpeningTime(hours) }} - {{ getClosingTime(hours) }}
        </li>
      </ul>

      <h3>Activity 12: Working with Arrays in Objects</h3>
      <p>Top Sellers:</p>
      <ul>
        <li
          v-for="(seller, index) in topSellers"
          :key="`seller-${index}`"
        >
          {{ getTopSellerTitle(seller) }}
        </li>
      </ul>

      <p>We operate in:</p>
      <ul>
        <li
          v-for="(location, index) in locations"
          :key="`location-${index}`"
        >
          {{ location }}
        </li>
      </ul>

      <p>
        Our #1 seller:
        <strong>{{ numberOneSeller }}</strong>
      </p>
    </section>

    <section class="lab-section">
      <h2>v-if & v-else</h2>
      <p>Toggle visibility based on a condition.</p>
      <button @click="showMessage = !showMessage">Toggle Message</button>
      <p class="message success">✨ You're a Vue superstar! ✨</p>
      <p>Click the button to see a message.</p>
    </section>

    <section class="lab-section">
      <h2>Attribute, Class and Style Binding with <code>v-bind</code></h2>
      <p>Highlighting Specific Authors:</p>
    </section>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import authors from '../assets/json/authors.json'
import bookstores from '../assets/json/bookstores.json'

const showMessage = ref(false)

// Activity 2: Get authors born after 1850
const modernAuthors = computed(() => {
  return authors.filter((author) => author.birthYear > 1850)
})

// Helper function for famous works
const getWorkTitle = (work) => {
  if (typeof work === 'string') {
    return work
  }

  return work.title || work.name || 'Untitled Work'
}

// Activity 3: Get all famous works
const allFamousWorks = computed(() => {
  return authors.flatMap((author) =>
    (author.famousWorks || []).map((work) => getWorkTitle(work))
  )
})

// Activity 4: Find author by name
const orwell = computed(() => {
  return authors.find((author) => author.name === 'George Orwell')
})

// Activity 5: Find author by ID
const austen = computed(() => {
  return authors.find((author) => Number(author.id) === 1)
})

const companyName = computed(() => {
  return (
    bookstores.companyName ||
    bookstores.company ||
    bookstores.name ||
    'Company name not provided'
  )
})

const totalStores = computed(() => {
  return (
    bookstores.totalStores ||
    bookstores.total ||
    bookstores.stores?.length ||
    'Total stores not provided'
  )
})

const storeTypes = computed(() => {
  return bookstores.storeTypes || []
})

const openingHours = computed(() => {
  return bookstores.openingHours || {}
})

const topSellers = computed(() => {
  return bookstores.topSellers || bookstores.bestSellers || []
})

const locations = computed(() => {
  return bookstores.locations || bookstores.cities || []
})

const numberOneSeller = computed(() => {
  const firstSeller = topSellers.value[0]

  if (!firstSeller) {
    return 'No seller provided'
  }

  return getTopSellerTitle(firstSeller)
})

const getStoreTypeName = (storeType) => {
  if (typeof storeType === 'string') {
    return storeType
  }

  return storeType.type || storeType.name || storeType.category || 'Unknown Type'
}

const getStoreTypeCount = (storeType) => {
  if (typeof storeType === 'string') {
    return 'N/A'
  }

  return storeType.count || storeType.total || storeType.numberOfStores || 'N/A'
}

const getOpeningTime = (hours) => {
  if (typeof hours === 'string') {
    return hours
  }

  return hours.open || hours.opening || hours.from || 'Open time not provided'
}

const getClosingTime = (hours) => {
  if (typeof hours === 'string') {
    return ''
  }

  return hours.close || hours.closing || hours.to || 'Close time not provided'
}

const getTopSellerTitle = (seller) => {
  if (typeof seller === 'string') {
    return seller
  }

  return seller.title || seller.name || seller.book || 'Unknown Seller'
}
</script>

<style scoped>
.json-lab {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  max-width: 80vw;
  margin: 0 auto;
  padding: 20px;
  background-color: #f4f4f4;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1,
h2 {
  color: #333;
}
h1 {
  text-align: center;
}

.lab-section {
  background-color: white;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.message {
  padding: 10px;
  border-radius: 5px;
  margin-top: 10px;
}

.success {
  background-color: #e7faf3;
  color: #42b883;
  border: 1px solid #42b883;
}

.highlight {
  background-color: #42b883;
}

code {
  background-color: #e0e0e0;
  padding: 2px 5px;
  border-radius: 4px;
  font-family: "Courier New", Courier, monospace;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  background-color: #f0f0f0;
  padding: 10px;
  margin: 5px 0;
  border-radius: 5px;
}
</style>