<template>
    <header class="header">
        <div class="companyTitle">Жилфонд</div>
        <div class="userName">Пользователь</div>
    </header>
</template>
  
<style lang="scss" scoped>
.header {
    position: relative;
    height: auto;
    max-width: 100%;
    padding: 50px 7.5% 0 7.5%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-family: 'Montserrat', sans-serif;

    .companyTitle {
        color: #E31F24;
        width: 171px;
        height: 39px;
        font-weight: 700;
        font-size: 32px;
        line-height: 39px;
        cursor: pointer;
    }

    .userName { 
        width: 117px;
        height: 20px;
        font-weight: 400;
        font-size: 16px;
        line-height: 19.5px;
        cursor: pointer;
    }
}  
</style>