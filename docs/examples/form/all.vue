<template>
  <el-card>
    <PlusForm
      v-model="state"
      label-width="140px"
      :columns="columns"
      @change="handleChange"
      @submit="handleSubmit"
      @submit-error="handleSubmitError"
      @reset="handleReset"
    />
  </el-card>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import type { PlusColumn, FieldValues } from 'plus-pro-components'

interface RestaurantItem {
  value: string
  link: string
}

interface Option {
  key: number
  label: string
  disabled: boolean
}

const state = ref<FieldValues>({
  autocomplete: 'vue',
  cascader: ['0', '0-0', '0-0-0'],
  'cascader-multiple': [
    ['0', '0-0', '0-0-0'],
    ['0', '0-0', '0-0-1'],
    ['0', '0-0', '0-0-2']
  ],
  checkbox: ['0'],
  'color-picker': 'rgba(255, 69, 0, 0.68)',
  year: '2024',
  years: ['2024', '2005'],
  month: '2024-02',
  date: '2024-03-05',
  dates: ['2024-03-05', '2024-03-06'],
  datetime: '2024-03-19 00:00:00',
  week: '2024-03-19',
  datetimerange: ['2024-03-07 00:00:00', '2024-03-09 00:00:00'],
  daterange: ['2024-02-29', '2024-03-29'],
  monthrange: ['2024-03', '2024-05'],
  select: '0',
  'select-multiple': ['0', '1'],
  input: '单行文本',
  textarea: '多行文本',
  'input-number': 4,
  rate: '3',
  switch: true,
  radio: '0',
  slider: 50,
  'time-picker': '2024-03-18 09:55:31',
  'time-select': '09:55:31',
  text: '文本',
  transfer: [1, 2, 3, 4, 5, 6],
  'plus-radio': 1,
  'plus-date-picker': ['2024-03-18 09:55:31', '2024-03-20 09:55:31'],
  'plus-input-tag': ['tag', 'tag1']
})

const generateData = () => {
  const data: Option[] = []
  for (let i = 1; i <= 15; i++) {
    data.push({
      key: i,
      label: `Option ${i}`,
      disabled: i % 4 === 0
    })
  }
  return data
}

const restaurants = ref<RestaurantItem[]>([])

const createFilter = (queryString: string) => {
  return (restaurant: RestaurantItem) => {
    return restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
  }
}
const loadAll = () => {
  return [
    { value: 'vue', link: 'https://github.com/vuejs/vue' },
    { value: 'element', link: 'https://github.com/ElemeFE/element' },
    { value: 'cooking', link: 'https://github.com/ElemeFE/cooking' },
    { value: 'mint-ui', link: 'https://github.com/ElemeFE/mint-ui' },
    { value: 'vuex', link: 'https://github.com/vuejs/vuex' },
    { value: 'vue-router', link: 'https://github.com/vuejs/vue-router' },
    { value: 'babel', link: 'https://github.com/babel/babel' }
  ]
}

restaurants.value = loadAll()

const cascaderOptions = [
  {
    value: '0',
    label: '陕西',
    children: [
      {
        value: '0-0',
        label: '西安',
        children: [
          {
            value: '0-0-0',
            label: '新城区'
          },
          {
            value: '0-0-1',
            label: '高新区'
          },
          {
            value: '0-0-2',
            label: '灞桥区'
          }
        ]
      }
    ]
  },
  {
    value: '1',
    label: '山西',
    children: [
      {
        value: '1-0',
        label: '太原',
        children: [
          {
            value: '1-0-0',
            label: '小店区'
          },
          {
            value: '1-0-1',
            label: '古交市'
          },
          {
            value: '1-0-2',
            label: '万柏林区'
          }
        ]
      }
    ]
  }
]

const columns: PlusColumn[] = [
  {
    label: 'autocomplete',
    width: 120,
    prop: 'autocomplete',
    valueType: 'autocomplete',
    tooltip: '自动补全输入框',
    fieldProps: {
      fetchSuggestions: (queryString: string, cb: any) => {
        const results = queryString
          ? restaurants.value.filter(createFilter(queryString))
          : restaurants.value
        // call callback function to return suggestions
        cb(results)
      }
    }
  },
  {
    label: 'cascader',
    prop: 'cascader',
    valueType: 'cascader',
    options: cascaderOptions
  },
  {
    label: 'cascader-multiple',
    prop: 'cascader-multiple',
    valueType: 'cascader',
    options: cascaderOptions,
    fieldProps: {
      props: { multiple: true }
    }
  },
  {
    label: 'checkbox',
    prop: 'checkbox',
    valueType: 'checkbox',
    options: [
      {
        label: '四六级',
        value: '0'
      },
      {
        label: '计算机二级证书',
        value: '1'
      },
      {
        label: '普通话证书',
        value: '2'
      }
    ]
  },
  {
    label: 'color-picker',
    prop: 'color-picker',
    valueType: 'color-picker'
  },
  {
    label: 'year',
    prop: 'year',
    valueType: 'date-picker',
    fieldProps: {
      type: 'year'
    }
  },
  {
    label: 'years',
    prop: 'years',
    valueType: 'date-picker',
    fieldProps: {
      type: 'years'
    }
  },
  {
    label: 'month',
    prop: 'month',
    valueType: 'date-picker',
    fieldProps: {
      type: 'month'
    }
  },
  {
    label: 'date',
    prop: 'date',
    valueType: 'date-picker',
    fieldProps: {
      type: 'date'
    }
  },
  {
    label: 'dates',
    prop: 'dates',
    valueType: 'date-picker',
    fieldProps: {
      type: 'dates'
    }
  },
  {
    label: 'datetime',
    prop: 'datetime',
    valueType: 'date-picker',
    fieldProps: {
      type: 'datetime'
    }
  },
  {
    label: 'week',
    prop: 'week',
    valueType: 'date-picker',
    fieldProps: {
      type: 'week'
    }
  },
  {
    label: 'datetimerange',
    prop: 'datetimerange',
    valueType: 'date-picker',
    fieldProps: {
      type: 'datetimerange'
    }
  },
  {
    label: 'daterange',
    prop: 'daterange',
    valueType: 'date-picker',
    fieldProps: {
      type: 'daterange'
    }
  },
  {
    label: 'monthrange',
    prop: 'monthrange',
    valueType: 'date-picker',
    fieldProps: {
      type: 'monthrange'
    }
  },
  {
    label: 'select',
    width: 120,
    prop: 'select',
    valueType: 'select',
    options: [
      {
        label: '未解决',
        value: '0',
        color: 'red'
      },
      {
        label: '已解决',
        value: '1',
        color: 'blue'
      },
      {
        label: '解决中',
        value: '2',
        color: 'yellow'
      },
      {
        label: '失败',
        value: '3',
        color: 'red'
      }
    ]
  },
  {
    label: 'select-multiple',
    width: 120,
    prop: 'select-multiple',
    valueType: 'select',
    options: [
      {
        label: '未解决',
        value: '0',
        color: 'red'
      },
      {
        label: '已解决',
        value: '1',
        color: 'blue'
      },
      {
        label: '解决中',
        value: '2',
        color: 'yellow'
      },
      {
        label: '失败',
        value: '3',
        color: 'red'
      }
    ],
    fieldProps: {
      multiple: true
    }
  },
  {
    label: 'input',
    width: 120,
    prop: 'input'
  },
  {
    label: 'input-number',
    prop: 'input-number',
    valueType: 'input-number',
    fieldProps: { precision: 2, step: 2 }
  },
  {
    label: 'textarea',
    prop: 'textarea',
    valueType: 'textarea',
    fieldProps: {
      maxlength: 10,
      showWordLimit: true,
      autosize: { minRows: 2, maxRows: 4 }
    }
  },
  {
    label: 'rate',
    width: 200,
    prop: 'rate',
    valueType: 'rate'
  },
  {
    label: 'switch',
    width: 100,
    prop: 'switch',
    valueType: 'switch'
  },
  {
    label: 'radio',
    prop: 'radio',
    valueType: 'radio',
    options: [
      {
        label: '诗',
        value: '0'
      },
      {
        label: '远方',
        value: '1'
      },
      {
        label: '美食',
        value: '2'
      }
    ]
  },
  {
    label: 'slider',
    prop: 'slider',
    valueType: 'slider'
  },
  {
    label: 'time-picker',
    prop: 'time-picker',
    valueType: 'time-picker'
  },
  {
    label: 'time-select',
    prop: 'time-select',
    valueType: 'time-select'
  },
  {
    label: 'text',
    prop: 'text',
    valueType: 'text'
  },
  {
    label: 'transfer',
    prop: 'transfer',
    valueType: 'transfer',
    fieldProps: {
      data: generateData()
    }
  },
  {
    label: 'divider',
    prop: 'divider',
    valueType: 'divider'
  },
  {
    label: 'tree-select',
    prop: 'tree-select',
    valueType: 'tree-select',
    fieldProps: {
      data: cascaderOptions
    }
  },
  {
    label: 'plus-radio',
    prop: 'plus-radio',
    valueType: 'plus-radio',
    options: [
      { label: '选项一', value: 1 },
      { label: '选项二', value: 2 }
    ]
  },
  {
    label: 'plus-date-picker',
    prop: 'plus-date-picker',
    valueType: 'plus-date-picker'
  },
  {
    label: 'plus-input-tag',
    prop: 'plus-input-tag',
    valueType: 'plus-input-tag'
  }
]

const handleChange = (values: FieldValues, prop: PlusColumn) => {
  console.log(values, prop, 'change')
}
const handleSubmit = (values: FieldValues) => {
  console.log(values, 'Submit')
}
const handleSubmitError = (err: any) => {
  console.log(err, 'err')
}
const handleReset = () => {
  console.log('handleReset')
}
</script>
