<template>
  <el-card>
    <el-row>
      <div class="el-step__icon is-text ms-api-col" style="float: left">
        <div class="el-step__icon-inner">{{controller.$treeNodeId}}</div>
      </div>
      <div>
        <el-row :gutter="10" type="flex" align="middle">
          <el-col :span="1">If</el-col>
          <el-col :span="6">
            <el-input size="small" v-model="controller.variable" :placeholder="$t('api_test.request.condition_variable')"/>
          </el-col>
          <el-col :span="5">
            <el-select v-model="controller.operator" :placeholder="$t('commons.please_select')" size="small"
                       @change="change">
              <el-option v-for="o in operators" :key="o.value" :label="$t(o.label)" :value="o.value"/>
            </el-select>
          </el-col>
          <el-col :span="6">
            <el-input size="small" v-model="controller.value" :placeholder="$t('api_test.value')" v-if="!hasEmptyOperator"/>
          </el-col>
          <el-col :span="4">
            <el-switch v-model="controller.enable" :inactive-text="$t('api_test.scenario.enable_disable')"/>
          </el-col>
          <el-col :span="2">
            <el-button size="mini" type="danger" icon="el-icon-delete" circle @click="remove"/>
          </el-col>
        </el-row>
      </div>
    </el-row>
  </el-card>
</template>

<script>
  export default {
    name: "MsIfController",
    props: {
      controller: {},
      node: {},
    },
    data() {
      return {
        operators: {
          EQ: {
            label: "commons.adv_search.operators.equals",
            value: "=="
          },
          NE: {
            label: "commons.adv_search.operators.not_equals",
            value: "!="
          },
          LIKE: {
            label: "commons.adv_search.operators.like",
            value: "=~"
          },
          NOT_LIKE: {
            label: "commons.adv_search.operators.not_like",
            value: "!~"
          },
          GT: {
            label: "commons.adv_search.operators.gt",
            value: ">"
          },
          LT: {
            label: "commons.adv_search.operators.lt",
            value: "<"
          },
          IS_EMPTY: {
            label: "commons.adv_search.operators.is_empty",
            value: "is empty"
          },
          IS_NOT_EMPTY: {
            label: "commons.adv_search.operators.is_not_empty",
            value: "is not empty"
          }
        }
      }
    },
    methods: {
      remove() {
        this.$emit('remove', this.controller, this.node);
      },
      change(value) {
        if (value.indexOf("empty") > 0 && !!this.controller.value) {
          this.controller.value = "";
        }
      }
    },
    computed: {
      hasEmptyOperator() {
        return !!this.controller.operator && this.controller.operator.indexOf("empty") > 0;
      }
    }
  }
</script>

<style scoped>
  .ms-api-col {
    background-color: #FCF6EE;
    border-color: #E6A23C;
    margin-right: 10px;
    color: #E6A23C;
  }
</style>
