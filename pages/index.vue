New-Sydney-Events-as-at-25-Aug-2021
<template>
  <div>
    <ul>
      <li v-for="(item, index) in articles" :key="index">
        <a :href="`/blog/${item.slug}`">{{ fmtDate(item.mydate) }}</a>
      </li>
    </ul>
  </div>
</template>

<script>
import dayjs from 'dayjs'

function compareValues(key, order) {
  order = order || 'desc'
  return function innerSort(a, b) {
    if (!a.hasOwnProperty(key) || !b.hasOwnProperty(key)) {
      // property doesn't exist on either object
      return 0
    }

    const varA = typeof a[key] === 'string' ? a[key].toUpperCase() : a[key]
    const varB = typeof b[key] === 'string' ? b[key].toUpperCase() : b[key]

    let comparison = 0
    if (varA > varB) {
      comparison = 1
    } else if (varA < varB) {
      comparison = -1
    }
    return order === 'desc' ? comparison * -1 : comparison
  }
}

export default {
  name: 'IndexPage',
  async asyncData({ $content, params }) {
    let articles = await $content('MD').fetch()
    articles = articles.slice().map((item) => {
      return {
        // name: item.slug.substr(25).replace(/=/g, ' '),
        mydate: dayjs(item.slug.substr(25)),
        slug: item.slug,
      }
    })
    articles = articles.sort(compareValues('mydate'))

    return {
      articles,
    }
  },
  methods: {
    fmtDate: function (val) {
      return dayjs(val).format('d MMM YYYY')
    },
  },
}
</script>
