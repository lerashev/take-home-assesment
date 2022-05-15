<template>
    <h1 class="main-header">
        The City of Futurpruf - Buildings' and Cell Towers' Data Table
    </h1>
    <ModalWindow
        v-show="showModal"
        @close="closeModalWindow"
        :cell-tower-closest="cellTowerClosest"
        :distance="closestDistance"
    />
    <input
        class="filter-input text-input"
        v-model="filterName"
        placeholder="Search by name"
    />

    <table class="table">
        <thead>
            <tr class="header">
                <th class="cell-header">Structure Type</th>
                <th class="cell-header">ID</th>
                <th class="cell-header">
                    <div>
                        <div>Name</div>
                        <div class="btn-sort" @click="handleSort">
                            sort
                            <div
                                :class="[
                                    'icon',
                                    sortDirection === 'A-Z'
                                        ? 'up'
                                        : sortDirection === 'Z-A'
                                        ? 'down'
                                        : '',
                                ]"
                            ></div>
                        </div>
                    </div>
                </th>
                <th class="cell-header">X Coordinate</th>
                <th class="cell-header">Y Coordinate</th>
            </tr>
        </thead>
        <tbody>
            <tr
                class="building-row"
                v-for="building in buildingsDataSorted"
                :key="building.id"
                @click="handler(building)"
            >
                <td class="cell cell-structure-type">
                    {{ building.structureType }}
                </td>
                <td class="cell cell-id">{{ building.id }}</td>
                <td class="cell cell-name">{{ building.name }}</td>
                <td class="cell cell-coordinates">
                    {{ building.x }}
                </td>
                <td class="cell cell-coordinates">
                    {{ building.y }}
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
const buildingsData = require("./buildingsData.json");
const cellTowersData = require("./cellTowersData.json");

import ModalWindow from "./components/ModalWindow.vue";

export default {
    name: "App",
    components: {
        ModalWindow,
    },
    data() {
        return {
            buildingsData,
            cellTowersData,
            showModal: false,
            filterName: "",
            sortDirection: "",
            cellTowerClosest: { name: "", x: null, y: null },
            closestDistance: null,
        };
    },
    methods: {
        displayModalWindow() {
            this.showModal = true;
        },
        closeModalWindow() {
            this.showModal = false;
        },

        handleSort() {
            if (this.sortDirection === "") {
                this.sortDirection = "A-Z";
                return;
            }
            if (this.sortDirection === "A-Z") {
                this.sortDirection = "Z-A";
                return;
            }
            this.sortDirection = "";
        },

        calculateDistance(p1, p2) {
            const deltaX = p1.x - p2.x;
            const deltaY = p1.y - p2.y;
            const distance = Math.sqrt(deltaX ** 2 + deltaY ** 2);
            return parseFloat(distance.toFixed(2));
        },

        handler(building) {
            const distances = this.cellTowersData.map((cellTower) =>
                this.calculateDistance(building, cellTower)
            );
            const closestDistance = Math.min(...distances);
            const cellTowerClosestIndex = distances.indexOf(closestDistance);
            this.cellTowerClosest = this.cellTowersData[cellTowerClosestIndex];
            this.closestDistance = closestDistance;
            this.displayModalWindow();
        },
    },

    computed: {
        buildingsDataFiltered() {
            return this.buildingsData.filter((building) => {
                return building.name
                    .toLowerCase()
                    .match(this.filterName.toLowerCase());
            });
        },
        buildingsDataSorted() {
            if (this.sortDirection === "A-Z") {
                return [...this.buildingsDataFiltered].sort((prev, cur) => {
                    return prev.name.localeCompare(cur.name);
                });
            }
            if (this.sortDirection === "Z-A") {
                return [...this.buildingsDataFiltered].sort((prev, cur) => {
                    return cur.name.localeCompare(prev.name);
                });
            }

            return this.buildingsDataFiltered;
        },
    },
};
</script>

<style>
html,
body {
    margin: 0;
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
}
.main-header {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 1.3rem;
    margin-top: 2rem;
}

.filter-input {
    --padding: 0.5rem;
    --width: 21%;
    display: block;
    width: var(--width);
    padding: var(--padding);
    margin-bottom: 0.5rem;
    border: 1px solid rgb(153, 153, 153);
    box-shadow: 0 0 3px 1px rgba(85, 84, 84, 0.233);
}

.header {
    background-color: rgba(219, 214, 206, 0.384);
}

.table {
    border-collapse: collapse;
    font-family: "Times New Roman", Times, serif;
    font-size: 1.2rem;
    box-shadow: 0 0 3px 1px rgba(85, 84, 84, 0.233);
    width: 100%;
}

.cell-header {
    border: 1px solid rgb(153, 153, 153);
    padding: 0.3rem 0.6rem;
    text-align: left;
}

.cell-header * {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

.building-row:hover {
    background-color: rgb(211, 211, 211);
}

.btn-sort {
    cursor: pointer;
    gap: 0.5rem;
}

.cell {
    padding: 0.3rem 0.6rem;
    border: 1px solid rgb(153, 153, 153);
    text-align: left;
}

.cell-coordinates {
    text-align: right;
}

.icon {
    width: 0;
    height: 0;
    border-top: 10px solid transparent;
    border-bottom: 10px solid transparent;
    border-right: 10px solid rgba(151, 149, 149, 0.938);
}

.icon.up {
    transform: rotate(90deg);
}

.icon.down {
    transform: rotate(-90deg);
}

@media (max-width: 620px) {
    .main-header {
        font-size: 1.2rem;
        text-align: center;
    }
    .table * {
        font-size: clamp(0.6rem, 3vw, 2rem);
    }
    .filter-input {
        width: clamp(8rem, 30%, 3rem);
        text-align: center;
        display: block;
        margin: 0.5rem auto;
    }
}
</style>
