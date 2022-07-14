<template>
    <div class="item">
        <h2> {{ title }} </h2>
        <div> {{ description }} </div> <br>
        <div class="strong"> {{ author }}</div>
        <div>{{ pubDate }}</div>
        <a :href="link">{{ link }}</a>
    </div>

</template>
<script>

export default {
    name: 'RssDetail',
    props: {
        item: Object,
    },
    data() {
        return {
            title: '',
            description: '',
            author: '',
            pubDate: '',
            link: ''
        }
    },
    computed: {
    },
    mounted() {
        this.item = this.$route.params.id
        this.title = this.item.title
        this.description = this.item.description
        this.author = this.item.author
        this.pubDate = this.item.pubDate
        this.link = this.item.link
    }
}
</script> 
<style >
.item {
    text-align: left;
    margin: 2em;

}

.strong {
    font-weight: bold;
}
</style>