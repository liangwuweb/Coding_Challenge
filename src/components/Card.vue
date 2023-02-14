<template>
    <div class="card-wrap" @mousemove="handleMouseMove" @mouseenter="handleMouseEnter" @mouseleave="handleMouseLeave"
        ref="card">
        <div class="card m-2" :style="cardStyle">
            <div class="card-body">
                <h5 class="fw-bold card-title">{{ item.name }}</h5>
                <ul class="list-group list-group-flush">
                    <li class="list-group-item">{{ item.brewery_type }}</li>
                    <li class="list-group-item">street: {{ item.street }}</li>
                    <li class="list-group-item">phone: {{ item.phone }}</li>
                </ul>

                <a target="_blank" :href="item.website_url != null ? item.website_url : '#'"
                    :class="item.website_url != null ? 'btn btn-primary my-btn' : 'd-none'"><span
                        class="text-2">website</span></a>
            </div>
        </div>
    </div>
</template>


<script>
export default {
    name: "Card",
    props: ["item"],
    data: function () {
        return {
            width: 0,
            height: 0,
            mouseX: 0,
            mouseY: 0,
            mouseLeaveDelay: null,
        };
    },
    computed: {
        mousePX: function () {
            return this.mouseX / this.width;
        },
        mousePY: function () {
            return this.mouseY / this.height;
        },
        cardStyle: function () {
            const rX = this.mousePX * 30;
            const rY = this.mousePY * -30;
            return {
                transform: `rotateY(${rX}deg) rotateX(${rY}deg)`,
            };
        },
    },
    methods: {
        handleMouseMove: function (e) {
            this.mouseX = e.pageX - this.$refs.card.offsetLeft - this.width / 2;
            this.mouseY = e.pageY - this.$refs.card.offsetTop - this.height / 2;
        },
        handleMouseEnter: function () {
            clearTimeout(this.mouseLeaveDelay);
        },
        handleMouseLeave: function () {
            this.mouseLeaveDelay = setTimeout(() => {
                this.mouseX = 0;
                this.mouseY = 0;
            }, 1000);
        },
    },
    mounted() {
        this.width = this.$refs.card.offsetWidth;
        this.height = this.$refs.card.offsetHeight;
    },

}
</script>

<style>
.card {
    position: relative;
    width: 240px;
    height: 300px !important;
    border-radius: 10px;
    box-shadow: #66666687 0 30px 60px 0;
    transition: 1s cubic-bezier(0.445, 0.05, 0.55, 0.95);
}

.card-wrap {
    margin-right: 10px;
    margin-left: 10px;
    transform: perspective(800px);
    transform-style: preserve-3d;
    cursor: pointer;
}

.card-wrap:hover .card {
    transition: 0.6s cubic-bezier(0.445, 0.05, 0.55, 0.95), box-shadow 2s cubic-bezier(0.23, 1, 0.32, 1) !important;
    border: 3px solid #f6cf34;
}

.card-title {
    color: #ca8a04;
}

.my-btn {
    background-color: #ca8a04 !important;
    border: 2px solid #976a09 !important;
    position: relative;
    overflow: hidden;
}

.my-btn,
.my-btn:before {
    transition: all 0.35s;
    transition-timing-function: cubic-bezier(0.31, -0.105, 0.43, 1.59);
}

.my-btn:before {
    content: "";
    width: 229%;
    height: 410%;
    position: absolute;
    transform: rotate(76deg);
    top: -118%;
    left: -230%;
    background: #e2ab35;
}

.my-btn:hover::before,
.my-btn:focus::before {
    top: -119%;
    left: -60%;
}


.text-2 {
    position: relative;
    font-weight: 600;
    transition: all 0.35s;
}
</style>