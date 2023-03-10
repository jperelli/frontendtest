<template>
  <div class="wrapper">
    <div class="board-wrapper" ref="boardWrapper">
      <div class="board" :style="{ height: `${boardSize}px`, width: `${boardSize}px` }">
        <div v-for="row in rowNames" :key="row" class="row">
          <div v-for="col in colNames" :key="col" class="col">
            <div
              class="cell"
              @click="handleCellClick(row, col)"
              :class="{
                active:
                  clicks.length > 0 &&
                  clicks[clicks.length - 1][0] == row &&
                  clicks[clicks.length - 1][1] == col
              }"
            >
              <div class="label">{{ row }}{{ col }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="drawer-wrapper">
      <div class="drawer">
        <h3>Clicks</h3>
        <div v-for="(click, i) in clicks">{{ i + 1 }}. {{ click[0] }} {{ click[1] }}</div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      rowNames: ['H', 'G', 'F', 'E', 'D', 'C', 'B', 'A'],
      colNames: [1, 2, 3, 4, 5, 6, 7, 8],
      clicks: [] as Array<[string, number]>,
      boardSize: 100
    }
  },
  mounted() {
    window.addEventListener('resize', this.resizeBoard)
    this.resizeBoard()
  },
  unmounted() {
    window.removeEventListener('resize', this.resizeBoard)
  },
  methods: {
    handleCellClick(row: string, col: number) {
      this.clicks.push([row, col])
    },
    resizeBoard() {
      const boardWrapper = this.$refs.boardWrapper as HTMLFormElement
      const h = boardWrapper.clientHeight - 2
      const w = boardWrapper.clientWidth - 2
      this.boardSize = Math.min(h, w)
      console.log('resize', this.boardSize)
    }
  }
}
</script>

<style>
.wrapper {
  display: flex;
  height: 100%;
  width: 100%;
  max-height: 100%;
  max-width: 100%;
}

.board-wrapper {
  flex-grow: 1;
  background-color: #304b2b;
  display: flex;
  align-items: center;
  justify-content: center;
}

.board {
  display: block;
}

.board .row {
  height: 12.5%;
  display: flex;
  flex-direction: row;
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 0;
}

.board .row .col {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 0;
}

.board .row .col .cell {
  display: inline-flex;
  height: 100%;
  background-color: #d18b47;
  justify-content: space-around;
}
.board .row .col .cell .label {
  align-self: center;
  color: #3335;
  font-size: 10px;
}

.board .row:nth-child(odd) .col:nth-child(even) .cell {
  background-color: #ffce9e;
}

.board .row:nth-child(even) .col:nth-child(odd) .cell {
  background-color: #ffce9e;
}

.board .row .col .cell:hover {
  background-color: #cce !important;
  box-shadow: 0px 0px 0px 2px #339 inset;
  border-radius: 2px;
  cursor: pointer;
}

.board .row .col .cell.active {
  box-shadow: 0px 0px 0px 6px #339 inset;
}

.drawer-wrapper {
  background-color: #fcfcfc;
}
.drawer {
  padding: 20px;
  line-height: 20px;
  overflow: auto;
  max-height: calc(100% - 40px);
  min-height: 200px;
}

@media (min-width: 800px) {
  .drawer-wrapper {
    height: 100%;
    width: 200px;
  }
}

@media (max-width: 800px) {
  .wrapper {
    flex-direction: column;
  }
  .drawer-wrapper {
    width: 100%;
    height: 200px;
  }
}
</style>
