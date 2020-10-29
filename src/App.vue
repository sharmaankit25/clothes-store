<template>
  <div id="app">
    <div class="grid-container">
      <div class="item2">
      <aside>
        <ProductsFilter @select="onFilterSelect" :filters="filters" />
      </aside>
      </div>
      <div class="item3">
        <div class="card">
            <div class="card-content flex-container">
            <span>{{ count }} Items</span>
            <span >
              <select name="sort" v-model="selectedSort" class="input">
                <option value="">Select Sort</option>
                <option v-for="option in sortOptions" :key="option.code" :value="option.code">
                  {{ option.label }}
                </option>
              </select>
            </span>
            </div>
        </div>
        <ProductsList :products="products" />
      </div>
    </div>
  </div>
</template>

<script>
import ProductsList from './components/ProductsList'
import ProductsFilter from './components/ProductsFilters'

export default {
  name: 'App',
  data () {
    return {
      products: [],
      filters: [],
      sortOptions: [],
      selectedSort: "",
      count: 0
    }
  },
  components: {
    ProductsList,
    ProductsFilter
  },
  methods: {
    onFilterSelect (selectedFilters) {
      let filterString = ''
      for(let key in selectedFilters) {
        filterString += `${key}-${selectedFilters[key]}`
      }
      this.fetchProducts(filterString)
    },
    async fetchProducts (activeFilters = null) {
      try {
        let res;
        if (activeFilters) {
          res = await this.$axios.get(`https://pim.wforwomanonline.com/pim/pimresponse.php/?service=category&store=1&url_key=top-wear-kurtas&page=1&count=20&sort_by=&sort_dir=desc&filter=${activeFilters}`)
        } else {
          res = await this.$axios.get('https://pim.wforwomanonline.com/pim/pimresponse.php/?service=category&store=1&url_key=top-wear-kurtas&page=1&count=20&sort_by=&sort_dir=desc&filter=')
          this.filters = res.data.result.filters
          this.sortOptions = res.data.result.sort
        }
        this.products = res.data.result.products
        this.count = res.data.result.count
      } catch (e) {
        console.log('Oops Something went wrong!!')
      }
    }
  },
  async mounted () {
    await this.fetchProducts()
  }
}
</script>

<style>

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #E4E4D8;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.row {
  width: 100%;
  margin: 2px;
  padding: 5px;
}

.float-left {
  float: left;
}

.float-right {
  float: right;
}


.grid-container {
  display: grid;
}

.button {
    background-color: #BB311D; /* Green */
    border-radius: 2px;
    border: none;
    color: white;
    padding: 5px 13px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 14px;
}

.button-block {
  width: 100%;
}

.button:hover {
    background-color: #1b581d; /* Green */
    border-radius: 2px;
    border: none;
    color: white;
    padding: 5px 14px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 14px;
    cursor: pointer;
}

.input {
  height: 30px;
  min-width: 80px;
}

ul.list {
    list-style-type: none;
    margin: 2px;
    padding: 2px;
}

.list li {
    padding: 6px;
    margin: 5px 0px 5px 0px;
    border-radius: 2px;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    transition: 0.3s;
    background-color: #ffffff;
    min-width: 30px;
}

.item1 { grid-area: header; }
.item2 { grid-area: menu; }
.item3 { grid-area: main; }
.item4 { grid-area: right; }
.item5 { grid-area: footer; }

.grid-container {
  display: grid;
  grid-template-areas:
    'header header header header header header'
    'menu main main main right right'
    'menu footer footer footer footer footer';
  grid-gap: 2px;
  padding: 5px;
}

.grid-container > div {
  text-align: center;
  padding: 5px 0;
}
</style>
