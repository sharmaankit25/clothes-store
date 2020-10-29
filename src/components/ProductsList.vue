<template>
<div>
    <div class="flex-container">
        <div class="flex-item" v-for="product in products" :key="product.id_product">
            <div class="card" @mouseenter="activeHoveredItem = product">
            <div class="card-content">
                <img class="product-image" :src="product.image" :alt="product.name" />
                <button v-if="activeHoveredItem && activeHoveredItem.id_product === product.id_product"
                    class="button button-block">View Details</button>
            </div>
            <div class="card-title">
                {{ product.name }}
            </div>
            <div class="card-content">
                <strike>₹ {{ product.price }}</strike>
                    <strong>₹ {{ product.price }}</strong>
                    <span class="discount">- {{ product.discount }} %</span>
                    <div v-if="activeHoveredItem && activeHoveredItem.id_product === product.id_product">
                        <span>Size</span> <span>{{ product.size }}</span>
                    </div>
            </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    data () {
        return {
            activeHoveredItem: null
        }
    },
    props: {
        products: Array
    }
}
</script>

<style>
    .discount {
        color: #BB311D;
        font-weight: 600;
        padding: 2px;
    }
    .product-image {
        height: 300px;
        width: 250px;
    }
    .flex-container {
        display: flex;
        flex-wrap: wrap;
        justify-content: flex-start;
        /* align-items: flex-start; */
        padding: 5px;
    }

    .flex-item {
        flex: 23%;
        max-width: 24%;
        padding: 0px 5px;
        margin: 2px;
    }

    .card {
        border-radius: 2px;
        box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
        transition: 0.3s;
        min-height: 50px;
        background-color: white;
    }

.card > .card-content {
    padding: 5px;
}

.card > .card-title {
    text-align: center;
    font-weight: 800;
    margin: 5px 0px 5px;
    padding: 2px;
}

.card > .card-footer {
    padding: 5px;
    text-align: right;
}

.card:hover {
    box-shadow: 0 8px 12px 0 rgba(0,0,0,0.2);
}

/* Create four equal columns that sits next to each other */
.column {
    flex: 23%;
    max-width: 24%;
    padding: 0px 5px;
    margin: 2px;
}

@media (max-width: 800px) {
    .flex-container {
      flex-direction: column;
    }
    .column {
        max-width: 100%;
        margin-bottom: 10px;
    }
}
</style>
