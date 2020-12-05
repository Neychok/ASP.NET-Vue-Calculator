<template>
  <div>
    <div class="about text-center py-8">
      <h1>Кредитен Калкулатор</h1>
      <p class="text-md-body-1">
        Пресметнете месечни вноски и ГПР (Годишен Процент на Разходите)
      </p>
    </div>
    <CreditForm></CreditForm>
  </div>
</template>

<script>
import CreditForm from "../components/CreditForm";
export default {
  components: { CreditForm }
};
</script>
