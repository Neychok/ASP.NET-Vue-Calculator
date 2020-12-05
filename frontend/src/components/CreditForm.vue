<template>
  <form class="mx-auto" style="max-width: 600px;">
    <template>
      <v-row>
        <v-col class="pa-2">
          <!-- Размер на кредит -->
          <v-text-field
            v-model="creditAmount"
            :error-messages="creditAmountErrors"
            outlined
            type="number"
            label="Размер на кредита"
            :counter="8"
            suffix="лв."
            required
            @input="$v.creditAmount.$touch()"
            @blur="$v.creditAmount.$touch()"
          >
          </v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col class="pa-2">
          <!-- Срок на кредит -->
          <v-text-field
            v-model="creditTerm"
            :error-messages="creditTermErrors"
            outlined
            type="number"
            label="Срок"
            :counter="3"
            required
            suffix="м."
            @input="$v.creditTerm.$touch()"
            @blur="$v.creditTerm.$touch()"
          />
        </v-col>
        <v-col class="pa-2">
          <!-- Лихва на кредит -->
          <v-text-field
            v-model="creditInterest"
            :error-messages="creditInterestErrors"
            outlined
            type="number"
            label="Лихва"
            :counter="4"
            required
            suffix="%"
            @input="$v.creditInterest.$touch()"
            @blur="$v.creditInterest.$touch()"
          />
        </v-col>
      </v-row>

      <v-row>
        <v-col class="pa-2" sm="12">
          <!-- Вид на вноски -->
          <v-select
            v-model="depositType"
            :error-messages="depositTypeErrors"
            :items="depositTypes"
            outlined
            label="Вид на вноска"
            required
            @input="$v.depositType.$touch()"
            @blur="$v.depositType.$touch()"
          />
        </v-col>
      </v-row>
    </template>
    <!-- Бутони -->
    <div class="d-flex justify-end">
      <v-btn
        :disabled="$v.$invalid"
        color="success"
        class="mr-4 mt-2"
        @click="validate"
      >
        Пресметни
      </v-btn>

      <v-btn color="error" class="mt-2" @click="reset">
        Изчисти полета
      </v-btn>
    </div>

    <!-- Допълнителни опции -->
    <p class="mt-4 mb-2 text-body-1">Допълнителни опции</p>

    <v-expansion-panels multiple focusable>
      <!-- Промоции -->
      <v-expansion-panel>
        <v-expansion-panel-header>Промоции</v-expansion-panel-header>
        <v-expansion-panel-content>
          <v-row class="pt-6">
            <v-col class="pa-1" cols="12" sm="6">
              <!-- Промоционална лихва -->
              <v-text-field
                v-model="promoInterest"
                :error-messages="promoInterestErrors"
                outlined
                suffix="%"
                label="Промо. лихва"
                type="number"
                @input="$v.promoInterest.$touch()"
                @blur="$v.promoInterest.$touch()"
              />
            </v-col>
            <v-col class="pa-1" cols="12" sm="6">
              <!-- Промоционален период -->
              <v-text-field
                v-model="promoTerm"
                :error-messages="promoTermErrors"
                outlined
                label="Промо. период"
                suffix="м."
                type="number"
                @input="$v.promoTerm.$touch()"
                @blur="$v.promoTerm.$touch()"
              />
            </v-col>
          </v-row>

          <v-row>
            <v-col class="pa-1" cols="12">
              <!-- Гратисен период -->
              <v-text-field
                v-model="gratis"
                :error-messages="gratisErrors"
                outlined
                label="Гратисен период"
                suffix="м."
                type="number"
                @input="$v.gratis.$touch()"
                @blur="$v.gratis.$touch()"
              />
            </v-col>
          </v-row>
        </v-expansion-panel-content>
      </v-expansion-panel>

      <!-- Такси -->
      <v-expansion-panel>
        <v-expansion-panel-header>Такси</v-expansion-panel-header>
        <v-expansion-panel-content>
          <!-- Първоначални такси -->
          <p class="text-center pt-6 subtitle text--secondary">
            Първоначални такси
          </p>
          <v-row cols="12">
            <v-col class="pa-1" cols="12" sm="6">
              <!-- Такса кандидатстване -->
              <v-text-field
                v-model="candidateFee"
                :error-messages="candidateFeeErrors"
                outlined
                label="Такса кандидатстване"
                type="number"
                @input="$v.candidateFee.$touch()"
                @blur="$v.candidateFee.$touch()"
              >
                <!-- Defaults to False "валута" -->
                <template v-slot:append>
                  <v-btn
                    @click="candidateFeeSelect = !candidateFeeSelect"
                    class="px-0"
                    min-width="36"
                  >
                    <v-icon v-if="candidateFeeSelect" medium dense>
                      mdi-percent-outline
                    </v-icon>
                    <div v-else class="body-2">
                      лв.
                    </div>
                  </v-btn>
                </template>
              </v-text-field>
            </v-col>
            <v-col class="pa-1" cols="12" sm="6">
              <!-- Такса обработка -->
              <v-text-field
                v-model="processingFee"
                :error-messages="processingFeeErrors"
                outlined
                label="Такса обработка"
                type="number"
                @input="$v.processingFee.$touch()"
                @blur="$v.processingFee.$touch()"
              >
                <!-- Defaults to True "процент" -->
                <template v-slot:append>
                  <v-btn
                    @click="processingFeeSelect = !processingFeeSelect"
                    class="px-0"
                    min-width="36"
                  >
                    <v-icon v-if="processingFeeSelect" medium dense>
                      mdi-percent-outline
                    </v-icon>
                    <div v-else class="body-2">
                      лв.
                    </div>
                  </v-btn>
                </template>
              </v-text-field>
            </v-col>
            <v-col class="pa-1" cols="12" sm="6">
              <!-- Други такси -->
              <v-text-field
                v-model="otherFee"
                :error-messages="otherFeeErrors"
                outlined
                label="Други такси"
                type="number"
                @input="$v.otherFee.$touch()"
                @blur="$v.otherFee.$touch()"
              >
                <!-- Defaults to False "валута" -->
                <template v-slot:append>
                  <v-btn
                    @click="otherFeeSelect = !otherFeeSelect"
                    class="px-0"
                    min-width="36"
                  >
                    <v-icon v-if="otherFeeSelect" medium dense>
                      mdi-percent-outline
                    </v-icon>
                    <div v-else class="body-2">
                      лв.
                    </div>
                  </v-btn>
                </template>
              </v-text-field>
            </v-col>
          </v-row>

          <v-divider class="mb-3" />

          <!-- Годишни такси -->
          <p class="text-center subtitle text--secondary">
            Годишни такси
          </p>
          <v-row>
            <v-col class="pa-1" cols="12" sm="6">
              <!-- Годишно управление -->
              <v-text-field
                v-model="annualProcessingFee"
                :error-messages="annualProcessingFeeErrors"
                outlined
                label="Годишнo управление"
                type="number"
                @input="$v.annualProcessingFee.$touch()"
                @blur="$v.annualProcessingFee.$touch()"
              >
                <!-- Defaults to True "процент" -->
                <template v-slot:append>
                  <v-btn
                    @click="
                      annualProcessingFeeSelect = !annualProcessingFeeSelect
                    "
                    class="px-0"
                    min-width="36"
                  >
                    <v-icon v-if="annualProcessingFeeSelect" medium dense>
                      mdi-percent-outline
                    </v-icon>
                    <div v-else class="body-2">
                      лв.
                    </div>
                  </v-btn>
                </template>
              </v-text-field>
            </v-col>
            <!-- Други годишни такси -->
            <v-col class="pa-1" cols="12" sm="6">
              <v-text-field
                v-model="otherAnnualFee"
                :error-messages="otherAnnualFeeErrors"
                outlined
                label="Други такси"
                type="number"
                @input="$v.otherAnnualFee.$touch()"
                @blur="$v.otherAnnualFee.$touch()"
              >
                <!-- Defaults to False "валута" -->
                <template v-slot:append>
                  <v-btn
                    @click="otherAnnualFeeSelect = !otherAnnualFeeSelect"
                    class="px-0"
                    min-width="36"
                  >
                    <v-icon v-if="otherAnnualFeeSelect" medium dense>
                      mdi-percent-outline
                    </v-icon>
                    <div v-else class="body-2">
                      лв.
                    </div>
                  </v-btn>
                </template>
              </v-text-field>
            </v-col>
          </v-row>

          <v-divider class="mb-3" />

          <!-- Месечни такси -->
          <p class="text-center subtitle text--secondary">
            Месечни такси
          </p>
          <v-row>
            <v-col class="pa-1" cols="12" sm="6">
              <!-- Месечна такса управление -->
              <div class="d-flex">
                <v-text-field
                  v-model="monthlyProcessingFee"
                  :error-messages="monthlyProcessingFeeErrors"
                  outlined
                  label="Месечнo управление"
                  type="number"
                  @input="$v.monthlyProcessingFee.$touch()"
                  @blur="$v.monthlyProcessingFee.$touch()"
                >
                  <!-- Defaults to True "процент" -->
                  <template v-slot:append>
                    <v-btn
                      @click="
                        monthlyProcessingFeeSelect = !monthlyProcessingFeeSelect
                      "
                      class="px-0"
                      min-width="36"
                    >
                      <v-icon v-if="monthlyProcessingFeeSelect" medium dense>
                        mdi-percent-outline
                      </v-icon>
                      <div v-else class="body-2">
                        лв.
                      </div>
                    </v-btn>
                  </template>
                </v-text-field>
              </div>
            </v-col>
            <v-col class="pa-1" cols="12" sm="6">
              <!-- Други месечни такси -->
              <v-text-field
                v-model="otherMonthlyFee"
                :error-messages="otherMonthlyFeeErrors"
                outlined
                label="Други такси"
                type="number"
                @input="$v.otherMonthlyFee.$touch()"
                @blur="$v.otherMonthlyFee.$touch()"
              >
                <!-- Defaults to False "лв." -->
                <template v-slot:append>
                  <v-btn
                    @click="otherMonthlyFeeSelect = !otherMonthlyFeeSelect"
                    class="px-0"
                    min-width="36"
                  >
                    <v-icon v-if="otherMonthlyFeeSelect" medium dense>
                      mdi-percent-outline
                    </v-icon>
                    <div v-else class="body-2">
                      лв.
                    </div>
                  </v-btn>
                </template>
              </v-text-field>
            </v-col>
          </v-row>
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
  </form>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, between, numeric } from "vuelidate/lib/validators";

export default {
  data: () => ({
    //
    creditAmount: "",
    creditTerm: "",
    creditInterest: "",
    depositType: null,
    depositTypes: ["Анюитетни вноски", "Намаляващи вноски"],
    //
    // Промоции
    promoInterest: "",
    promoTerm: "",
    gratis: "",
    //
    // Такси
    // Първоначални такси
    // Процент - True
    // Валута - False
    candidateFee: "",
    candidateFeeSelect: false,
    processingFee: "",
    processingFeeSelect: true,
    otherFee: "",
    otherFeeSelect: false,
    //
    //Годишни такси
    annualProcessingFee: "",
    annualProcessingFeeSelect: true,
    otherAnnualFee: "",
    otherAnnualFeeSelect: false,
    //
    // Месечни такси
    monthlyProcessingFee: "",
    monthlyProcessingFeeSelect: true,
    otherMonthlyFee: "",
    otherMonthlyFeeSelect: false,
  }),

  mixins: [validationMixin],

  validations: {
    creditAmount: { required, between: between(100, 10000000), numeric },
    creditTerm: { required, between: between(0, 960), numeric },
    creditInterest: { required, between: between(0, 1000), numeric },
    depositType: { required },
    //
    promoInterest: { between: between(0, 1000), numeric },
    promoTerm: {
      between(value) {
        return between(0, this.creditTerm)(value);
      },
      numeric,
    },
    gratis: {
      between(value) {
        return between(0, this.creditTerm)(value);
      },
      numeric,
    },
    //
    candidateFee: {
      numeric,
      between(value) {
        return between(0, this.creditAmount)(value);
      },
    },
    processingFee: {
      numeric,
      between(value) {
        return between(0, this.creditAmount)(value);
      },
    },
    otherFee: {
      numeric,
      between(value) {
        return between(0, this.creditAmount)(value);
      },
    },
    //
    annualProcessingFee: { numeric, between },
    otherAnnualFee: { numeric, between },
    //
    monthlyProcessingFee: { numeric, between },
    otherMonthlyFee: { numeric, between },
  },

  computed: {
    //Валидация
    // Стандартни полета
    creditAmountErrors() {
      const errors = [];
      if (!this.$v.creditAmount.$dirty) return errors;
      !this.$v.creditAmount.required &&
        errors.push("Моля, въведете размер на кредита");
      !this.$v.creditAmount.between &&
        errors.push("Моля, въведете валиден размер на кредит (от 100 до 10М)");
      return errors;
    },
    creditTermErrors() {
      const errors = [];
      if (!this.$v.creditTerm.$dirty) return errors;
      !this.$v.creditTerm.required &&
        errors.push("Моля, въведете срок на кредита");
      !this.$v.creditTerm.between &&
        errors.push("Моля, въведете валиден срок (до 960 месеца)");
      return errors;
    },
    creditInterestErrors() {
      const errors = [];
      if (!this.$v.creditInterest.$dirty) return errors;
      !this.$v.creditInterest.required &&
        errors.push("Моля, въведете размера на лихвата");
      !this.$v.creditInterest.between &&
        errors.push("Моля, въведете валиден размер на лихва (до 1000%)");
      return errors;
    },
    depositTypeErrors() {
      const errors = [];
      if (!this.$v.depositType.$dirty) return errors;
      !this.$v.depositType.required &&
        errors.push("Моля, изберете вид на вноската");
      return errors;
    },

    // Промоции
    promoInterestErrors() {
      const errors = [];
      if (!this.$v.promoInterest.$dirty) return errors;
      !this.$v.promoInterest.between &&
        errors.push("Моля, въведете валиден размер на лихва (до 1000%)");
      return errors;
    },
    promoTermErrors() {
      const errors = [];
      if (!this.$v.promoTerm.$dirty) return errors;
      !this.$v.promoTerm.between &&
        errors.push(
          "Промо. период не може да бъде повече от периода на кредита"
        );
      return errors;
    },
    gratisErrors() {
      const errors = [];
      if (!this.$v.gratis.$dirty) return errors;
      !this.$v.gratis.between &&
        errors.push(
          "Гратисният период не може да бъде повече от периода на кредита"
        );
      return errors;
    },

    // Такси
    // Първоначални такси
    candidateFeeErrors() {
      const errors = [];
      if (!this.$v.candidateFee.$dirty) return errors;
      !this.$v.candidateFee.between &&
        errors.push(
          "Гратисният период не може да бъде повече от периода на кредита"
        );
      return errors;
    },
    processingFeeErrors() {
      const errors = [];
      if (!this.$v.processingFee.$dirty) return errors;
      !this.$v.processingFee.between &&
        errors.push(
          "Гратисният период не може да бъде повече от периода на кредита"
        );
      return errors;
    },
    otherFeeErrors() {
      const errors = [];
      if (!this.$v.otherFee.$dirty) return errors;
      !this.$v.otherFee.between &&
        errors.push(
          "Гратисният период не може да бъде повече от периода на кредита"
        );
      return errors;
    },
    //
    // Годишни такси
    annualProcessingFeeErrors() {
      const errors = [];
      if (!this.$v.annualProcessingFee.$dirty) return errors;
      !this.$v.annualProcessingFee.between &&
        errors.push(
          "Гратисният период не може да бъде повече от периода на кредита"
        );
      return errors;
    },
    otherAnnualFeeErrors() {
      const errors = [];
      if (!this.$v.otherAnnualFee.$dirty) return errors;
      !this.$v.otherAnnualFee.between &&
        errors.push(
          "Гратисният период не може да бъде повече от периода на кредита"
        );
      return errors;
    },
    //
    // Месечни такси
    monthlyProcessingFeeErrors() {
      const errors = [];
      if (!this.$v.monthlyProcessingFee.$dirty) return errors;
      !this.$v.monthlyProcessingFee.between &&
        errors.push(
          "Гратисният период не може да бъде повече от периода на кредита"
        );
      return errors;
    },
    otherMonthlyFeeErrors() {
      const errors = [];
      if (!this.$v.otherMonthlyFee.$dirty) return errors;
      !this.$v.otherMonthlyFee.between &&
        errors.push(
          "Гратисният период не може да бъде повече от периода на кредита"
        );
      return errors;
    },
  },

  methods: {
    validate() {
      this.$v.$touch();
    },
    reset() {
      this.creditAmount = "";
      this.creditTerm = "";
      this.creditInterest = "";
      this.depositType = null;
      // Промоции
      this.promoInterest = "";
      this.promoTerm = "";
      this.gratis = "";
      //Такси
      // Първоначални такси
      this.candidateFee = "";
      this.processingFee = "";
      this.otherFee = "";
      //Годишни такси
      this.annualProcessingFee = "";
      this.otherAnnualFee = "";
      // Месечни такси
      this.monthlyProcessingFee = "";
      this.otherMonthlyFee = "";
      this.$v.$reset();
    },
  },
};
</script>

<style>
.v-application--is-ltr .v-text-field .v-input__append-inner {
  margin-top: 0px;
  align-self: center;
}
</style>
