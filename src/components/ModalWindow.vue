<template>
    <div class="modal-overlay" @click.self="$emit('close')">
        <div class="modal">
            <h3>Closest Cell Tower:</h3>
            <div class="cell-tower-name">{{ cellTowerClosest.name }}</div>
            <div class="cell-tower-coordinates">
                {{ "x:" }} {{ cellTowerClosest.x }}, {{ "y:" }}
                {{ cellTowerClosest.y }}
            </div>
            <div class="cell-tower-distance">{{ distance }} {{ "meters" }}</div>
            <button class="close-modal-btn" @click="$emit('close')">
                Close
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: "ModalWindow",
    props: {
        cellTowerClosest: Object,
        distance: Number,
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.modal-overlay {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: center;
    background-color: rgba(32, 31, 31, 0.164);
}

.modal {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 1.5rem;
    background-color: rgb(255, 255, 255);
    height: 20rem;
    width: 20rem;
    margin-top: 15%;
    padding: 2rem 0;
    border: 1px solid rgb(153, 153, 153);
    font-family: "Times New Roman", Times, serif;
    font-size: 1.3rem;
}

.close-modal-btn {
    background: none;
    border: 1px solid rgb(128, 128, 128);
    width: 40%;
    cursor: pointer;
    border: 1px solid rgb(153, 153, 153);
    box-shadow: 0 0 3px 1px rgba(85, 84, 84, 0.233);
    padding: 0.3rem;
}

@media (max-width: 620px) {
    .modal {
        height: 15rem;
        width: 15rem;
        font-size: 1rem;
    }
}
</style>
