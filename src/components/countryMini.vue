<template>
    <div class="parent" @click="$emit('clicked', countryName)">
        <div class="flag" :style="{ backgroundImage: `url('${flag}')` }"></div>
        <ul>
            <li>
                <h3>{{ countryName }}</h3>
            </li>
            <li>
                <p>
                    <strong>Population:</strong>
                    {{ population }}
                </p>
            </li>
            <li>
                <p>
                    <strong>Region:</strong>
                    {{ region }}
                </p>
            </li>
            <li>
                <p>
                    <strong>Capital:</strong>
                    {{ capital }}
                </p>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: "countryMini",
    props: {
        flag: String,
        countryName: String,
        population: String,
        region: String,
        capital: String
    }
};
</script>

<style scoped>
div.parent {
    display: grid;
    justify-items: flex-start;
    grid-template-rows: 1fr 1fr;
    border-radius: 10px;
    width: 300px;
    box-shadow: 0 2px 6px 4px rgb(233, 233, 233);
    cursor: pointer;
}

ul {
    list-style: none;
    padding: 1.25rem 0.5rem 2.5rem 2rem;
}

li {
    margin-bottom: 0.45rem;
}

li:first-child {
    margin-bottom: 1rem;
}

h3 {
    font-size: 1.3rem;
}

p {
    font-size: 15px;
}

strong {
    font-weight: 700;
}

.flag {
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    width: 100%;
    background-size: 100% 100%;
    background-repeat: no-repeat;
    background-position: center center;
}
</style>
