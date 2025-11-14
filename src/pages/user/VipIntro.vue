<script setup lang="ts">
import BasePage from '@/components/BasePage.vue'
import BaseButton from '@/components/BaseButton.vue'
import {useRouter} from 'vue-router'
import {useUserStore} from '@/stores/auth.ts'
import {User} from "@/apis/user.ts";
import {onMounted} from "vue";
import Header from "@/components/Header.vue";
import {LevelBenefits, levelBenefits} from "@/apis/member.ts";

const router = useRouter()
const userStore = useUserStore()

interface Plan {
  id: string
  name: string
  price: number
  unit: '月' | '年'
  desc: string
  highlight?: string
  autoRenew?: boolean
  features: string[]
}


const member = $computed<User['member']>(() => userStore.user?.member ?? {} as any)

const memberEndDate = $computed(() => {
  if (member?.endDate === null) return '永久'
  return member?.endDate
})

// Get current plan info
const currentPlan = $computed(() => {
  if (!member?.active) return null
  return plans.find(p => p.id === member.planId) || null
})

// Toggle auto-renewal
function toggleAutoRenew() {
  // TODO: Implement API call to toggle auto-renewal
  console.log('Toggle auto-renewal:', !member.autoRenew)
}


// Get button text based on current plan
function getPlanButtonText(plan: Plan) {
  if (!member?.active) return '选择'
  if (plan.id === currentPlan?.id) return '当前计划'

  // Compare prices to determine upgrade/downgrade
  if (plan.price > (currentPlan?.price || 0)) return '升级'
  return '降级'
}

function goPurchase(plan: Plan) {
  return router.push('/pay')

  if (!userStore.isLogin) {
    router.push({path: '/login', query: {redirect: '/vip'}})
    return
  }

  if (plan.id === currentPlan?.id) return

  router.push('/user')
}

let data = $ref<LevelBenefits>({} as any)
onMounted(async () => {
  let res = await levelBenefits({levelCode: 'basic'})
  if (res.success) {
    data = res.data
  }
})

const plans: Plan[] = $computed(() => {
  let list = []
  if (data?.level) {
    list.push({
      id: 'monthly',
      name: '月付',
      price: data.level.price,
      unit: '月',
      desc: '',
    },)
    list.push({
      id: 'monthly-auto',
      name: '连续包月',
      price: data.level.price_auto,
      unit: '月',
      desc: '',
      highlight: '性价比更高',
      autoRenew: true,
    },)
    list.push({
      id: 'monthly',
      name: '年度会员',
      price: data.level.yearly_price,
      unit: '年',
      highlight: '年度优惠',
    },)
  }
  return list
})

</script>

<template>
  <BasePage>
    <div class="space-y-6">
      <div>
        <Header title="会员介绍"></Header>
        <div class="center">
          <div>
            <div class="text-lg flex items-center" v-for="f in data.benefits" :key="f.name">
              <IconFluentCheckmarkCircle20Regular class="mr-2 text-green-600"/>
              <span>{{ f.name }}</span>
            </div>
          </div>
        </div>
      </div>

      <div class="flex justify-between">
        <div class="title">选择会员级别并立即开始试用。您可以随时升级、降级或取消。</div>
        <div class="subtitle">三种方案，按需选择</div>
      </div>

      <div class="plans">
        <div v-for="p in plans" :key="p.id"
             class="card bg-reverse-white shadow-lg p-0 shadow-lg overflow-hidden flex flex-col">
          <div class="plan-name">{{ p.name }}</div>
          <div class="p-6 flex flex-col justify-between flex-1">
            <div class="plan-head">
              <div class="price">
                <span class="amount">¥{{ p.price }}</span>
                <span class="unit">/ 每{{ p.unit }}</span>
              </div>
              <div class="desc">{{ p.desc }}</div>
              <div v-if="p.highlight" class="tag">{{ p.highlight }}</div>
            </div>
            <div v-if="p.autoRenew" class="text-sm flex items-center mt-4">
              <IconFluentArrowRepeatAll20Regular class="mr-2"/>
              开启自动续费，可随时关闭
            </div>
            <BaseButton
              class="w-full mt-4"
              size="large"
              :type="p.id === currentPlan?.id ? 'primary' : 'info'"
              :disabled="p.id === currentPlan?.id"
              @click="goPurchase(p)">
              {{ getPlanButtonText(p) }}
            </BaseButton>
          </div>
        </div>
      </div>

      <!-- Membership Status Display -->
      <div v-if="member?.active" class="card bg-green-50 border border-green-200 mt-3 mb-6 shadow-lg">
        <div class="flex items-center justify-between">
          <div class="flex items-center">
            <IconFluentCheckmarkCircle20Regular class="mr-2 text-green-600"/>
            <div>
              <div class="font-semibold text-green-800">当前计划：{{ member?.levelDesc }}</div>
              <div class="text-sm text-green-600">
                到期时间：{{ memberEndDate }}
              </div>
            </div>
          </div>
          <div class="text-align-end space-y-2">
            <div v-if="member.autoRenew" class="flex items-center gap-space">
              <div class="flex items-center text-sm text-gray-600">
                <IconFluentArrowRepeatAll20Regular class="mr-1"/>
                <span>自动续费已开启</span>
              </div>
              <BaseButton
                size="small"
                type="info"
                @click="toggleAutoRenew">
                关闭
              </BaseButton>
            </div>
          </div>
        </div>
      </div>
    </div>
  </BasePage>
</template>

<style scoped lang="scss">

.plans {
  display: grid;
  gap: 3rem;
  grid-template-columns: repeat(3, minmax(0, 1fr));
}

.plan-head {
  @apply flex flex-col gap-2;
}

.plan-name {
  @apply text-2xl font-bold bg-gray-300 px-6 py-4;
}

.price {
  @apply flex items-end gap-1;
}

.amount {
  @apply text-4xl font-500;
}

.unit {
  @apply text-base text-gray-500;
}

.desc {
  @apply text-sm text-gray-600;
}

.tag {
  @apply text-xs bg-yellow-100 text-yellow-700 px-2 py-1 rounded w-fit;
}
</style>

