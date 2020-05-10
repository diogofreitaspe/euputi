<template>
  <v-app id="inspire">
    <v-container style="margin-bottom: 64px">
      <div class="title mb-4">Escore Unificado para Priorização</div>
      <div class="title">SOFA</div>
      <div class="caption">Sequential Organ Failure Assessment</div>
      <div v-for="q in sofa_questions" :key="q.title">
        <v-radio-group v-model="q.value" hide-details>
          <div class="body-2 mb-2">{{q.title}}</div>
          <v-radio v-for="opt in q.options" :label="opt.text" :value="opt.value" />
        </v-radio-group>
      </div>
      <div class="my-3">SOFA: {{sofa}}</div>
      <v-divider class="my-4" />
      <div class="title">CFS</div>
      <div class="caption">Escala de fragilidade</div>
      <v-radio-group v-model="cfs_score" hide-details>
        <v-radio v-for="item in cfs_items" :key="item.text" :label="item.text" :value="item.value" />
      </v-radio-group>
      <v-divider class="my-4" />
      <div class="title">ICC</div>
      <div class="caption">Índice de comorbidade de Charlson</div>
      <v-checkbox
        v-for="item in icc_items"
        v-model="icc_selected"
        :label="`${item.value} - ${item.text}`"
        :value="item"
        class="my-1"
        hide-details
      />
      <v-divider class="my-4" />
      <div class="title">KPS</div>
      <div class="caption">Karnofsky Performance Status</div>
      <v-radio-group v-model="kps_score" hide-details>
        <v-radio v-for="item in kps_items" :key="item.text" :label="`${item.value} - ${item.text}`" :value="item.value" />
      </v-radio-group>
    </v-container>
    <v-layout class="eup" row align-center>
      <div>EUP: {{eup_score}}</div>
      <v-spacer />
      <v-btn @click="clear" small flat>Limpar</v-btn>
    </v-layout>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      sofa_questions: [
        {
          title: 'Neurológico: Escala de Coma de Glasgow',
          hint: '',
          value: 0,
          options: [
            {text: 'Normal', value: 0},
            {text: '13 a 14', value: 1},
            {text: '10 a 12', value: 2},
            {text: '6 a 9', value: 3},
            {text: '<6', value: 4}
          ]
        },
        {
          title: 'Cardiovascular: Hipotensão, mmHG',
          value: 0,
          options: [
            {text: 'Normal', value: 0},
            {text: 'PAM < 70 mmHG', value: 1},
            {text: 'Dopamina ≤ 5 ou Dobutamina qualquer dose', value: 2},
            {text: 'Dopamina > 5 ou Noraepinefrina ≤ 0,1', value: 3},
            {text: 'Dopamina > 15 ou Noraepinefrina > 0,1', value: 4}
          ]
        },
        {
          title: 'Respiratório: Saturação Periférica de O2 (SpO2 > 92%)',
          value: 0,
          hint: '',
          options: [
            {text: 'Normal', value: 0},
            {text: 'O2 até 2l/min cateter nasal ', value: 1},
            {text: 'O2 até 5l/min cateter nasal', value: 2},
            {text: 'FiO2 até 40% ventilação mecânica', value: 3},
            {text: 'FiO2 > 40% ventilação mecânica', value: 4}
          ]
        },
        {
          title: 'Coagulação: Plaquetas 10³/μl',
          value: 0,
          options: [
            {text: 'Normal', value: 0},
            {text: '< 20', value: 4},
            {text: '< 50', value: 3},
            {text: '< 100', value: 2},
            {text: '< 150', value: 1}
          ]
        },
        {
          title: 'Hepático: INR | Inspeção',
          value: 0,
          options: [
            {text: 'Normal', value: 0},
            {text: '<1,1 | anictérico', value: 1},
            {text: '1,1 - 1,36', value: 2},
            {text: '1,36 - 1,88', value: 3},
            {text: '1,88 - 2,15 | ictérico', value: 4}
          ]
        },
        {
          title: 'Renal: Creatinina mg/dL | Diurese mL/dia',
          value: 0,
          options: [
            {text: 'Normal', value: 0},
            {text: '1,2 - 1,9 | >500', value: 1},
            {text: '2 - 2,4', value: 2},
            {text: '3,5 - 4,9 | <500', value: 3},
            {text: '>5,0 | <200', value: 4}
          ]
        },
      ],
      cfs_score: 1,
      cfs_items: [
        {
          value: 1,
          text: 'CFS < 4: Muito ativo, ativo ou regular.'
        },
        {
          value: 2,
          text: 'CFS 4: Vunerável. Sintomas limitam atividade mas não depende dos outros para ajuda diária.'
        },
        {
          value: 3,
          text: 'CFS 5-6: Levemente ou moderadamente frágil.'
        },
        {
          value: 4,
          text: 'CFS 7-8: Muito ou severamente frágil.'
        }
        // {
        //   value: 1,
        //   text: 'Muito ativo: Robustas, ativos, com energia e motivadas.'
        // },
        // {
        //   value: 2,
        //   text: 'Ativo: Nenhum sintoma ativo de doença.'
        // },
        // {
        //   value: 3,
        //   text: 'Regular: Problemas de saúde bem controlados.'
        // },
        // {
        //   value: 4,
        //   text: 'Vulnerável: Sintomas limitam atividade mas não depende dos outros para ajuda diária.'
        // },
        // {
        //   value: 5,
        //   text: 'Levemente frágil: Lentidão evidente e precisam de ajuda para atividades instrumentais de vida diária mais complexas.'
        // },
        // {
        //   value: 6,
        //   text: 'Moderadamente frágil: Precisam de ajuda em todas as atividades externas e manutenção da casa'
        // },
        // {
        //   value: 7,
        //   text: 'Muito frágil: Completamente dependente para cuidades pessoais (física ou cognitiva). Sem alto risco de morte em 6 meses.'
        // },
        // {
        //   value: 8,
        //   text: 'Severamente frágil: Completamente dependentes, incapazes de se recuperar de uma doença leve.'
        // },
      ],
      icc_selected: [],
      icc_items: [
        {text: 'Infarto do miocárdio', value: 1},
        {text: 'Insuficiência cardíaca congestiva', value: 1},
        {text: 'Doença vascular periférica', value: 1},
        {text: 'Doença do cérebro - vascular', value: 1},
        {text: 'Demência', value: 1},
        {text: 'Doença pulmonar crônica', value: 1},
        {text: 'Doença do tecido conjuntivo', value: 1},
        {text: 'Úlcera', value: 1},
        {text: 'Doença hepática crônica ou cirrose', value: 1},
        {text: 'Diabetes sem complicação', value: 1},
        {text: 'Hemiplegia ou paraplegia', value: 2},
        {text: 'Diabetes com complicação', value: 2},
        {text: 'Doença renal severa ou moderada', value: 2},
        {text: 'Tumor maligno', value: 2},
        {text: 'Leucemia', value: 2},
        {text: 'Linfoma', value: 2},
        {text: 'Doença do fígado severa ou moderada', value: 3},
        {text: 'Tumor sólido metastático', value: 6},
        {text: 'SIDA', value: 6}
      ],
      kps_score: 1,
      kps_items: [
        {
          value: 1,
          text: 'Com ou sem doença crônica, consegue trabalhar normalmente'
        },
        {
          value: 2,
          text: 'Com doença crônica, consegue trabalhar apesar de ter sintomas'
        },
        {
          value: 3,
          text: 'Não consegue trabalhar mas mantém hobbies e autocuidado'
        },
        {
          value: 4,
          text: 'É incapaz de cuidar de si mesmo'
        }
      ],
    }),
    computed: {
      sofa () {
        return this.sofa_questions.reduce((total, item) => total + item.value, 0)
      },
      sofa_score () {
        if (this.sofa >= 6 && this.sofa <= 9) {
          return 2
        }
        if (this.sofa >= 10 && this.sofa <= 12) {
          return 3
        }
        if (this.sofa >= 13) {
          return 4
        }
        return 1
      },
      icc_cfs_score () {
        const icc_score = this.icc_selected.reduce((total, item) => total + item.value, 0)
        if (icc_score === 2 || this.cfs_score === 2) {
          return 2
        }
        if ([3, 4, 5].indexOf(icc_score) >= 0 || this.cfs_score === 3) {
          return 3
        }
        if (icc_score >= 6 || this.cfs_score === 4) {
          return 4
        }
        return 1
      },
      eup_score () {
        return this.sofa_score + this.kps_score + this.icc_cfs_score
      }
    },
    methods: {
      clear () {
        this.sofa_questions.forEach(q => q.value = 0)
        this.kps_score = 1
        this.icc_selected = []
        this.cfs_score = 0
      }
    }
  }
</script>

<style>
.eup {
  background-color: white;
  border-top: 1px solid #e5e5e5;
  font-weight: bold;
  font-size: 16px;
  width: 100%;
  padding: 8px;
  position: fixed;
  bottom: 0;
}
</style>
