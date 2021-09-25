<template>
  <div class="check">
    <div class="check__wrapper">
      <label for="check">
        <input type="checkbox" name="check" checked />
        {{ part }} рублей <span>{{ numberYear }}</span>
        <hr />
      </label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Checkbox',
  props: ['part', 'numberYear']
};
</script>

<style lang="scss">
.check {
  &__wrapper {
    display: flex;
    flex-direction: column;
    line-height: 100%;
  }
}

input[type='checkbox'] {
  accent-color: #ea0029;
  width: 20px;
  height: 20px;
  transform: translateY(25%);
  margin-right: 11px;
  cursor: pointer;
}

hr {
  margin: 16px 0;
  background: #dfe3e6;
}

span {
  font-weight: 400;
  font-size: 14px;
  line-height: 24px;
  color: #808080;
}
</style>
