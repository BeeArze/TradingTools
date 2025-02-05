<template>
  <van-form @submit="calculate">
    <van-cell-group inset>
      <van-field
        v-model="openPrice"
        label="开仓价格"
        type="number"
        step="0.000001"
        :rules="numberRules"
        clearable
      />
      <van-field
        v-model="stopLossPrice"
        label="止损价格"
        type="number"
        step="0.000001"
        :rules="numberRules"
        clearable
      />
      <van-field
        v-model="stopLossAmount"
        label="止损金额"
        type="number"
        step="0.000001"
        :rules="numberRules"
        clearable
      />
    </van-cell-group>
    <div style="margin: 16px;">
      <van-button round block type="info" native-type="submit" :disabled="!isFormValid">计算</van-button>
      <van-button round block type="default" native-type="reset" style="margin-top: 10px;">重置</van-button>
    </div>
    <div v-if="result !== null" class="result">
      可开仓数量: {{ result }}
    </div>
    <div v-if="error" class="error">{{ error }}</div>
  </van-form>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';
import BigNumber from 'bignumber.js';

export default defineComponent({
  name: 'TradeForm',
  setup() {
    const openPrice = ref<string>('');
    const stopLossPrice = ref<string>('');
    const stopLossAmount = ref<string>('');
    const result = ref<string | null>(null);
    const error = ref<string | null>(null);

    // 定义数字验证规则
    const numberRules = [
      {
        required: true,
        message: '此字段不能为空'
      },
      {
        validator: (value: string, rule: any) => {
          if (!value) return '此字段不能为空';
          const bn = new BigNumber(value);
          if (bn.isLessThanOrEqualTo(0)) return '请输入大于零的数';
          if (value.includes('.') && value.split('.')[1].length > 6) return '小数点后最多6位';
          return true;
        },
        message: ''
      }
    ];

    // 计算逻辑
    const calculate = () => {
      if (isFormValid.value) {
        const op = new BigNumber(openPrice.value);
        const slp = new BigNumber(stopLossPrice.value);
        const sla = new BigNumber(stopLossAmount.value);
        const diff = op.minus(slp);
        if (diff.isGreaterThan(0)) {
          result.value = sla.dividedBy(diff).toFixed(6);
          error.value = null;
        } else {
          result.value = null;
          error.value = "开仓价格必须大于止损价格。";
        }
      } else {
        result.value = null;
        error.value = "请确保输入的数值大于零，且开仓价格大于止损价格。";
      }
    };

    // 表单验证是否有效
    const isFormValid = computed(() => {
      const op = new BigNumber(openPrice.value || '0');
      const slp = new BigNumber(stopLossPrice.value || '0');
      const sla = new BigNumber(stopLossAmount.value || '0');
      return op.isGreaterThan(0) &&
             slp.isGreaterThan(0) &&
             sla.isGreaterThan(0) &&
             op.isGreaterThan(slp);
    });

    return {
      openPrice,
      stopLossPrice,
      stopLossAmount,
      numberRules,
      isFormValid,
      calculate,
      result,
      error
    };
  }
});
</script>

<style scoped>
.result, .error {
  margin-top: 16px;
  text-align: center;
}

.error {
  color: red;
}
</style>
