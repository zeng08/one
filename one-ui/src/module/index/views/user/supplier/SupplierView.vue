<template>
  <section class="content">
    <div class="row">
      <div class="col-md-12">
        <div class="nav-tabs-custom">
          <ul class="nav nav-tabs">
            <li :class="{active : step == 1}" @click="step = 1"><a>基础信息</a></li>
            <li :class="{active : step == 2}" @click="step = 2"><a>审核记录</a></li>
            <li :class="{active : step == 3}" @click="step = 3"><a>项目记录</a></li>
            <li :class="{active : step == 4}" @click="step = 4"><a>调查记录</a></li>
          </ul>
          <div class="tab-content">
            <div class="tab-pane active">
              <BaseInfo v-if="step == 1" :obj="obj" :supplierId="id" type="1"/>
              <AuditLog v-if="step == 2" :obj="obj" :supplierId="id"/>
              <Biddings v-if="step == 3" :obj="obj" :supplierId="id"/>
              <QuestionnaireInfo v-if="step == 4" :obj="obj" :supplierId="id"/>
            </div>
          </div>
          <div class="box-footer">
            <div class="col-md-2 col-sm-0"></div>
            <div class="col-md-2 col-sm-2">
              <button class="btn btn-block btn-default" @click="$router.go(-1)">返回</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import BaseInfo from './info/BaseInfo'
import AuditLog from './info/AuditLog'
import Biddings from './info/Biddings'
import QuestionnaireInfo from './info/QuestionnaireInfo'

export default {
  props: {
    supplierId: {}
  },
  components: {
    BaseInfo,
    AuditLog,
    Biddings,
    QuestionnaireInfo
  },
  data: function () {
    return {
      obj: {},
      step: 1
    }
  },
  computed: {
    id: function () {
      if (this.supplierId) {
        return this.supplierId
      } else {
        return this.$route.params.id
      }
    }
  },
  mounted () {
    this.load()
  },
  methods: {
    load () {
      if (this.id) {
        this.$api.user.getSupplier(this.id).then((response) => {
          let result = response.data
          if (result.ok && result.data) {
            this.obj = result.data
          }
        })
      }
    }
  }
}
</script>
