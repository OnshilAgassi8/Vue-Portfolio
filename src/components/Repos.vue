<script setup>
    import PrevIcon from './icons/prevIcon.vue';
    import NextIcon from './icons/nextIcon.vue';
    import Skeleton from './Skeleton.vue'
</script>

<script>

export default {

data() {
return {
searchQuery: '',
items: [
{ id: 1, name: 'Item 1' },
{ id: 2, name: 'Item 2' },
{ id: 3, name: 'Another Item' }
]
};
},
computed: {
filteredItems() {
if (!this.searchQuery) return this.items;
return this.items.filter(item =>
item.name.toLowerCase().includes(this.searchQuery.toLowerCase())
);
}
},

        data() {
            return {
                repoData: [],
                page: 1,
                view: "",
                currentPage: 1,
                loading: false,
                perPage: 6,
                skeleton: [...new Array(6)]

            }
        },
        methods: {
            fetchData: function() {
                this.loading = true
                fetch(`https://api.github.com/users/OnshilAgassi8/repos`, {
                headers: {
                    Accept: "application/json"
                },
            }) 
            .then((res) => res.json()) 
            .then((data) => {
                this.repoData = this.repoData.concat(data)
                this.loading = false
            })
            },
            prevPage () {
                if (this.currentPage === 1) {
                    return
                }else {

                    this.currentPage--
                }
            },
            nextPage () {
                this.currentPage++
            },
            
        },
        mounted() {
            this.fetchData()
        },
        computed: {
            showMore: function() {
                let start = (this.currentPage - 1) * this.perPage
                let end = start + this.perPage
                this.loading = false
                return this.repoData.slice(start, end)
            },

            lastPage: function() {
                let length = this.repoData.length
                return length / this.perPage
            }
        }
        
    }
</script>

<template>

    <div>
        <input type="text" v-model="searchQuery" placeholder="Search by repository name" @input="performSearch">

        <select class="select-btn">
            <option value>All Languages</option>
            <option value="HTML">HTML</option>
            <option value="CSS">CSS</option>
            <option value="JavaScript">JavaScript</option>
        </select>

        <ul>
            <li v-for="item in filtedItems" :key="item.id">{{item.name}}</li>
        </ul>
        
        
    </div>

    <div>
        <div class="repo-container"> 
            <Skeleton v-if="loading" v-for="n in skeleton">{{ skeleton }}</Skeleton>  
            <div v-else v-for="repo in showMore" class="repo-card" :key="repo.id">
                <router-link :to="`/details/${repo.name}`"><h2 class="repo-name">{{ repo.name }}</h2></router-link>
                <p class="language">Language: {{ repo.language }}</p>
                <p class="date">Start date & time: {{ repo.created_at }}</p>
                <p class="visibility">Visibility: {{ repo.visibility }}</p>
            </div>
           
        </div>
        <div class="pagination">
            <button class="view-more" :class=" { 'disabled' : currentPage === 1 } " @click="prevPage"><PrevIcon /></button>
            <p class="current-page">{{ currentPage }}</p>
            <button class="view-more" :class=" {'disabled' : currentPage === 2 } " @click="nextPage" :disabled="currentPage === 2"><NextIcon /></button>
        </div>

    </div>
            

</template>


<style>

input[type="text"] {
    width: 25%;
    padding: 9px;
    margin-bottom: 40px;
    margin-left: 50px;
    margin-right: 10px;
    border: 1px solid #d479a3;
    border-radius: 5px;
    box-sizing: border-box;
}

input::placeholder {
    color: #000000;
    font-style: italic;
    font-weight: bold;
  }
  
select.select-btn {
    width: 11%;
    padding: 9px;
    margin-bottom: 40px;
    border: 1px solid #d479a3;
    border-radius: 5px;
    font-weight:bold;
    font-style: italic;
}

.repo-container {
    display: grid;
    grid-template-columns: repeat(auto-fit,minmax(300px,1fr));
    grid-gap: 30px;
    width: 90%;
    margin: 0 auto;
    margin-bottom: 5rem;
}

.repo-card,
.repodetail-card {
    border: 1px solid #5e3749;
    padding: 13px;
    border-radius: 30px;
}


.repo-name {
    color: #d479a3;
    font-size: 2rem;
    word-break: break-word;
}

p {
    padding: 5px 0;
}

.pagination {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    margin: 3rem 0;
}

button {
    background: none;
    border: none;
    cursor: pointer;
}

.view-more svg {
    width: 2rem;
}

.disabled {
    cursor: not-allowed;
    opacity: 0.5;
}



</style>