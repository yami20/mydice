<template>
  <div class="container">
    <p>入力した選択肢からランダムに1つを抽選してくれます。</p>
    <p>「保存する」を押すとローカルストレージに保存され、同一端末から繰り返し利用できるようになります。</p>
    <div class="buttons" id="dices" v-for="(items, name) in dices" :key="name">
      <b-button type="is-info" v-on:click="selectDice(name)">{{ name }}</b-button>
    </div>

    <div id="edit">
      <form v-on:submit.prevent="onSubmit">
        <b-field label="選択肢">
          <textarea v-model='currentItems' class="textarea" :rows="currentItems.split(/\n/).length" />
        </b-field>
        <b-button type="is-info" v-on:click.prevent="roll">抽選する</b-button>
        <b-field label="保存するときのなまえ">
          <b-input v-model='currentName'></b-input>
        </b-field>
      </form>
      <b-button type="is-success" v-on:click.prevent="update">保存する</b-button>
      <b-button type="is-warning" v-on:click.prevent="unset">削除する</b-button>
      <p v-bind:result='result'>{{ result }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'List',
  data() {
    return {
      currentName: '',
      currentItems: '',
      result: '',
      dices: {}
    }
  },
  mounted() {
    if (localStorage.dices) {
      this.dices = JSON.parse(localStorage.getItem('dices'))
    }
  },
  methods: {
    update() {
      this.$set(this.dices, this.currentName, this.currentItems)
      localStorage.setItem('dices', JSON.stringify(this.dices))
    },
    roll() {
      if (this.currentItems == undefined) { return }
      var candidates = this.currentItems.split('\n');
      this.result = candidates[Math.floor(Math.random() * candidates.length)]
    },
    unset() {
      this.$delete(this.dices, this.currentName)
      this.currentItems = undefined
      this.currentName = undefined
      localStorage.setItem('dices', JSON.stringify(this.dices))
    },
    selectDice (name) {
      this.currentName = name
      this.currentItems = this.dices[name]
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
