<template>
  <div class="item">

    <div class="item__part question">
      <div class="prefix">Q</div>
      <div class="body">
        <div  class="body__content">
          {{dialog.q.content}}
        </div>
        <div  class="body__date">
          {{qDate}}
        </div>
      </div>
    </div>
    
    <div class="item__part answer">
      <div class="prefix">A</div>
      <div class="body">
        <div  class="body__content">
          {{dialog.a.content}}
        </div>
        <div  class="body__date">
          {{aDate}}
        </div>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: 'item',
  props: {
    dialog: {
      type: Object,
      required: true
    }
  },
  data() {
    const options = {
      hour: 'numeric',
      minute: 'numeric',
      second: 'numeric'
    };

    return {
      qDate: (new Date(this.dialog.q.date)).toLocaleString('ru', options),
      aDate: (new Date(this.dialog.a.date)).toLocaleString('ru', options) 
    }
  }
}
</script>

<style scoped>
.item {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
} 
.item__part {
  max-width: 90%;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
}
.item__part.question {
  align-self: flex-start;
  margin-bottom: 5px;
}
.item__part.answer {
  align-self: flex-end;
  margin-bottom: 10px;
}
.question .prefix {
  background-color: #f99;
}
.answer .prefix {
  background-color: #9f9;
}
.prefix {
  width: 25px;
  min-width: 25px;
  height: 25px;
  margin-left: 5px;
  margin-right: 5px;
  border-radius: 50%;
  font-size: 18px;
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center; 
}
.body {
  min-height: 25px;
  margin-left: 5px;
  margin-right: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-radius: 10px 10px 10px 0;
  background-color: #fff;
  font-size: 14px;
  color: #555;
  font-weight: normal;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center; 
}
.body__content {
  align-self: flex-start;
}
.body__date {
  margin-top: 3px;
  font-size: 12px;
  font-style: italic;
  color: #aaa;
  align-self: flex-end;
}
</style>
