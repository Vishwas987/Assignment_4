<template>
    <header id="header">
      <div id="header-title">
        Onboarding Tasks
      </div>
      <nav id="header-nav">
        <button class="mdc-button mdc-button--outlined mdc-button--icon-leading" @click="$emit('open-add-form')">
          <span class="mdc-button__ripple"></span>
          <i class="material-icons mdc-button__icon" aria-hidden="true">add</i>
          <span class="mdc-button__label">Add Task</span>
        </button>
      </nav>
    </header>
</template>

<script>
export default{
    name: 'AppHeader'
}
</script>

<style scoped>
#header{
  padding-bottom: 0.5rem;
  display: flex;
  
}
#header-title{
  width: 50%;
  font-size: 18px;
  font-weight: 600;
  display: flex;
  align-items: flex-end;
}
#header-nav{
  display: flex;
  width: 50%;
  justify-content: right;
}
</style>
