<template>
    <div class="parent">
        <button @click="$emit('returnBack')" :class="{ 'dark-mode': darkMode }">
            <i class="fa fa-arrow-left"></i> Back
        </button>
        <div class="theGrid">
            <img :src="currentCountry.flag" alt />
            <div class="flex">
                <div>
                    <h1 :class="{ 'dark-mode': darkMode }">
                        {{ currentCountry.name }}
                    </h1>
                    <div class="grid-content">
                        <ul
                            class="grid-contents-left"
                            :class="{ 'dark-mode': darkMode }"
                        >
                            <li>
                                <p>
                                    <strong>Native Name:</strong>
                                    {{ currentCountry.nativeName }}
                                </p>
                            </li>
                            <li>
                                <p>
                                    <strong>Population:</strong>
                                    {{ currentCountry.population }}
                                </p>
                            </li>
                            <li>
                                <p>
                                    <strong>Region:</strong>
                                    {{ currentCountry.region }}
                                </p>
                            </li>
                            <li v-if="currentCountry.subregion != ''">
                                <p>
                                    <strong>Sub Region:</strong>
                                    {{ currentCountry.subregion }}
                                </p>
                            </li>
                            <li v-if="currentCountry.capital != ''">
                                <p>
                                    <strong>Capital:</strong>
                                    {{ currentCountry.capital }}
                                </p>
                            </li>
                        </ul>
                        <ul
                            class="grid-contents-right"
                            :class="{ 'dark-mode': darkMode }"
                        >
                            <li>
                                <p>
                                    <strong>Top Level Domain:</strong>
                                    {{
                                        currentCountry.topLevelDomain.join(", ")
                                    }}
                                </p>
                            </li>
                            <li>
                                <p>
                                    <strong>Currencies:</strong>
                                    {{ currentCountry.currencies.join(", ") }}
                                </p>
                            </li>
                            <li>
                                <p>
                                    <strong>Languages:</strong>
                                    {{ currentCountry.languages.join(", ") }}
                                </p>
                            </li>
                        </ul>
                    </div>
                </div>
                <div
                    class="border-countries"
                    v-if="currentCountry.borderCountries.length !== 0"
                    :class="{ 'dark-mode': darkMode }"
                >
                    <p>
                        <strong class="border-title">Border Countries:</strong>
                    </p>
                    <span
                        class="border-country"
                        v-for="country in currentCountry.borderCountries"
                        @click="$emit('changeCountry', country)"
                        :class="{ 'dark-mode': darkMode }"
                        :key="country"
                        >{{ country }}</span
                    >
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "countryView",
    props: {
        currentCountry: Object,
        darkMode: Boolean
    }
};
</script>

<style scoped>
.parent {
    margin-top: 4rem;
}
.theGrid {
    margin-top: 3rem;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 5rem;
}

.flex {
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.grid-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
}

img {
    width: 100%;
}

.flex h1 {
    font-size: 1.95rem;
    margin-bottom: 1.5rem;
}

ul {
    list-style: none;
}

li {
    margin-bottom: 1rem;
}
p {
    font-size: 14px;
    font-weight: 300;
}
.border-countries {
    display: flex;
    align-items: baseline;
    margin-top: 1.5rem;
    flex-wrap: wrap;
}

span.border-country {
    cursor: pointer;
    border: 1px solid rgb(192, 192, 192);
    padding: 0.15rem 1.25rem;
    border-radius: 5px;
    font-size: 14px;
    font-weight: 300;
    margin-right: 0.5rem;
    margin-bottom: 0.5rem;
}

.border-title {
    margin-right: 0.5rem;
}

button {
    padding: 0.5rem 2rem;
    color: hsl(0, 0%, 20%);
}

i {
    margin-right: 0.5rem;
}

h1.dark-mode,
ul.dark-mode,
.border-countries.dark-mode {
    color: white;
}

.border-country.dark-mode {
    background-color: hsl(209, 23%, 22%);
    box-shadow: 0 2px 4px 2px rgba(10, 10, 10, 0.473);
}

@media screen and (max-width: 630px) {
    .parent {
        margin-top: 2rem;
    }

    .theGrid {
        grid-template-columns: 1fr;
        grid-row-gap: 1.5rem;
    }

    .grid-content {
        grid-template-columns: 1fr;
        grid-row-gap: 1rem;
    }
}
</style>
