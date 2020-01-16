<template>
    <div id="app">
        <header>
            <div class="container">
                <div class="inside">
                    <h1>Where in the world?</h1>
                    <h2 id="text-mode">
                        <i class="fa fa-moon-o" aria-hidden="true"></i>
                        Dark Mode
                    </h2>
                </div>
            </div>
        </header>
        <div class="container">
            <transition name="fadeAll" mode="out-in">
                <div v-if="showAllCountries">
                    <div class="inside flexstart">
                        <input
                            type="text"
                            placeholder="Search for a country..."
                            v-model="countryName"
                        />
                        <div class="filter-holder">
                            <button @click="showFilterList = !showFilterList">
                                Filter by Region
                                <i class="fa fa-chevron-right"></i>
                            </button>
                            <transition name="fade">
                                <ul v-if="showFilterList">
                                    <li>Africa</li>
                                    <li>America</li>
                                    <li>Asia</li>
                                    <li>Europe</li>
                                    <li>Oceania</li>
                                </ul>
                            </transition>
                        </div>
                    </div>
                    <div class="country-holder">
                        <country-mini
                            v-for="(country, index) in filteredCountries"
                            :key="country + index"
                            :flag="country.flag"
                            :population="country.population"
                            :countryName="country.name"
                            :region="country.region"
                            :capital="country.capital"
                            @clicked="switchModes"
                        ></country-mini>
                    </div>
                </div>
                <country-view
                    :currentCountry="currentCountry"
                    @returnBack="showAllCountries = true"
                    v-else
                ></country-view>
            </transition>
        </div>
    </div>
</template>

<script>
import countryMini from "./components/countryMini.vue";
import countryView from "./components/countryView.vue";
var countries = require("i18n-iso-countries");

export default {
    name: "app",
    components: {
        countryMini,
        countryView
    },
    data() {
        return {
            showAllCountries: true,
            showFilterList: false,
            myCountries: [],
            countryName: "",
            currentCountry: {
                name: "Afghanistan",
                nativeName: "افغانستان",
                population: "27,657,145",
                region: "Asia",
                subRegion: "Southern Asia",
                capital: "Kabul",
                flag: "https://restcountries.eu/data/afg.svg",
                topLevelDomain: [".af"],
                currencies: ["Afghan afghani"],
                languages: ["Pashto", "Uzbek", "Turkmen"],
                borderCountries: ["IRN", "PAK", "TKM", "UZB", "TJK", "CHN"]
            }
        };
    },
    methods: {
        formatNumber(num) {
            return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1,");
        },
        matchName(name) {
            return name.toLowerCase().includes(this.countryName.toLowerCase());
        },
        switchModes(countryName) {
            const newCountry = this.myCountries.find(
                country => country.name === countryName
            );
            if (newCountry != null) {
                this.currentCountry = newCountry;
                this.showAllCountries = false;
            } else {
                console.log("ERROR");
            }
        }
    },
    computed: {
        filteredCountries() {
            return this.myCountries.filter(country =>
                this.matchName(country.name)
            );
        }
    },
    created() {
        countries.registerLocale(require("i18n-iso-countries/langs/en.json"));
        fetch("https://restcountries.eu/rest/v2/all")
            .then(res => (res.ok ? res.json() : console.log("error")))
            .then(data => {
                data.forEach(country => {
                    let aCountry = {
                        name: country.name,
                        population: this.formatNumber(country.population),
                        region: country.region,
                        capital: country.capital,
                        flag: country.flag,
                        nativeName: country.nativeName,
                        subregion: country.subregion,
                        topLevelDomain: country.topLevelDomain //array
                    };
                    aCountry.currencies = [];
                    country.currencies.forEach(currency =>
                        aCountry.currencies.push(currency.name)
                    );
                    aCountry.languages = [];
                    country.languages.forEach(language =>
                        aCountry.languages.push(language.name)
                    );
                    aCountry.borderCountries = [];
                    country.borders.forEach(borderCountry =>
                        aCountry.borderCountries.push(
                            countries.getName(borderCountry, "en")
                        )
                    );
                    this.myCountries.push(aCountry);
                });
            })
            .catch(err => console.log(err));
    }
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

h1 {
    font-size: 1.5rem;
    color: hsl(200, 15%, 8%);
}

h2 {
    font-size: 1.1rem;
    font-weight: 600;
    color: hsl(200, 15%, 8%);
}

header {
    width: 100%;
    padding: 1.5rem 0;
    box-shadow: 0 -4px 12px 0px rgb(136, 136, 136);
    margin-bottom: 2rem;
}

body {
    font-family: "Nunito Sans", sans-serif;
    background-color: hsl(0, 0%, 98%);
}

.container {
    padding: 0 5rem;
}

.filter-holder {
    position: relative;
}

.inside {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.flexstart {
    align-items: flex-start;
}

.inside.flexstart {
    margin-top: 3rem;
}

.inside.flexstart ul {
    position: absolute;
    width: 100%;
    list-style: none;
    padding: 0.75rem 0;
    margin-top: 0.25rem;
    box-shadow: 0 2px 4px 2px rgb(238, 238, 238);
    background-color: hsl(0, 0%, 98%);
    font-family: "Nunito Sans", sans-serif;
    font-size: 14px;
    border-radius: 5px;
    z-index: 1;
}

.inside.flexstart ul li {
    padding: 0.25rem 5rem 0.25rem 1rem;
    transition: background-color 0.2s;
    width: 100%;
    cursor: pointer;
}

.inside.flexstart ul li:hover {
    background-color: hsl(0, 0%, 92%);
}

input[type="text"] {
    display: inline-block;
    padding: 1.25rem;
    padding-left: 4.5rem;
    font-family: "Nunito Sans", sans-serif;
    font-size: 14px;

    box-shadow: 0 0 7px 3px rgb(238, 238, 238);
    border: 0;
    width: 100%;
    max-width: 30rem;
    border-radius: 5px;
    color: hsl(0, 0%, 52%);
}

button {
    border-radius: 5px;
    position: relative;
    padding: 1.25rem 5rem 1.25rem 1rem;
    font-family: "Nunito Sans", sans-serif;
    font-size: 14px;
    background-color: white;
    border: 0;
    box-shadow: 0 0 6px 3px rgb(238, 238, 238);
    cursor: pointer;
    white-space: nowrap;
    z-index: 2;
}

.fa-chevron-right {
    position: absolute;
    top: 50%;
    right: 0;
    margin-right: 1rem;
    font-size: 12px;
    transform: rotate(0deg) translate(0, -50%);
}

.fade-enter-active,
.fade-leave-active {
    transform: translateY(0);
    transition: all 0.4s ease-out;
    pointer-events: all;
}
.fade-enter,
.fade-leave-to {
    transform: translateY(-20px);
    opacity: 0;
    pointer-events: none;
}

.fadeAll-enter-active,
.fadeAll-leave-active {
    transition: all 0.4s ease-out;
    pointer-events: all;
}

.fadeAll-enter,
.fadeAll-leave-to {
    opacity: 0;
    pointer-events: none;
}

.country-holder {
    margin-top: 3.5rem;
    margin-bottom: 1rem;
    display: grid;
    justify-items: center;
    grid-template-columns: repeat(6, 1fr);
    grid-row-gap: 15rem;
    grid-column-gap: 2rem;
}

#text-mode {
    cursor: pointer;
}

@media screen and (max-width: 2250px) {
    .country-holder {
        grid-template-columns: repeat(5, 1fr);
    }
}

@media screen and (max-width: 1900px) {
    .country-holder {
        grid-template-columns: repeat(4, 1fr);
    }
}

@media screen and (max-width: 1450px) {
    .country-holder {
        grid-template-columns: repeat(3, 1fr);
    }
}

@media screen and (max-width: 1050px) {
    .country-holder {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media screen and (max-width: 800px) {
    .country-holder {
        grid-template-columns: 1fr;
    }
}
</style>
