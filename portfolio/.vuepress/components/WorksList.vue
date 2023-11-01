<template>
  <div>
    <!-- Filtre par catégorie -->
    <select v-model="selectedCategory">
      <option value="">Toutes les catégories</option>
      <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
    </select>

    <div class="project-list">
      <router-link
          :to="post.path"
          tag="div"
          v-for="post in filteredPosts"
          :key="post.title"
          class="post"
          :style="{ backgroundImage: `url(${post.frontmatter.thumbnail})` }"
      >

        <div class="info">
          <h2>{{ post.frontmatter.title }}</h2>
          <span v-if="post.frontmatter.description">{{ post.frontmatter.description }}</span>
        </div>

      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedCategory: ''
    };
  },
  computed: {
    categories() {
      const allCategories = this.$site.pages.map(page => page.frontmatter.categories);
      return Array.from(new Set(allCategories));
    },
    filteredPosts() {
      return this.$site.pages
          .filter(x => {
            if (this.selectedCategory && x.frontmatter.categories !== this.selectedCategory) {
              return false;
            }
            return x.path.startsWith('/works/') && !x.frontmatter.works_index;
          })
          .sort((a, b) => new Date(b.frontmatter.date) - new Date(a.frontmatter.date));
    }
  }
}
</script>


<style scoped>

  .post {
    position: relative;
    width: 100%;
    height: 70vh;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    margin-bottom: 5vw;
    cursor: pointer;
  }

  .info {
    position: absolute;
    left: 0;
    top: 2rem;
    padding: 0.5rem 1rem;
    background: rgba(255,255,255, 1);
    max-width: 400px;
  }

  .info h2 {
    display: inline-block;
    width: auto;
    font-size: 0.8rem;
    font-weight: 700;
    margin: 0;
  }

  .info span {
    display: inline-block;
    width: auto;
    margin: 0;
    margin-left: 0.5rem;
    font-size: 0.8rem;
  }

  select {
    display: block;
    width: 100%;
    max-width: 300px;
    margin: 1rem 0;
    padding: 0.5rem 1rem;
    font-size: 1rem;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
    appearance: none;
  }

  select:focus {
    border-color: #80bdff;
    outline: 0;
    box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
  }


  select::-ms-expand {
    display: none;
  }

  select::after {
    content: "\25BC";
    position: absolute;
    top: 50%;
    right: 0.5rem;
    transform: translateY(-50%);
    pointer-events: none;
  }
</style>
