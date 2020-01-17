<template>
    <div
        class="parent"
        @click="$emit('clicked', countryName)"
        :class="{ 'dark-mode': darkMode }"
    >
        <div class="flag" :style="{ backgroundImage: `url('${flag}')` }"></div>
        <ul :class="{ 'dark-mode': darkMode }">
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
            <li v-if="capital != ''">
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
        capital: String,
        darkMode: Boolean
    }
};
</script>

<style scoped>
div.parent {
    display: grid;
    justify-items: flex-start;
    grid-template-rows: 1fr 1fr;
    border-radius: 10px;
    width: 100%;
    max-width: 300px;
    box-shadow: 0 2px 6px 4px rgb(233, 233, 233);
    cursor: pointer;
}

div.parent.dark-mode {
    box-shadow: 0 2px 4px 2px rgba(10, 10, 10, 0.473);
}

div.parent.dark-mode,
ul.dark-mode li {
    background-color: hsl(209, 23%, 22%);
    color: white;
}

ul {
    list-style: none;
    padding: 1.25rem 0.5rem 2.5rem 1.25rem;
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
