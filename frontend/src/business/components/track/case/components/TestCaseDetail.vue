<template>
  <el-form :model="form" ref="caseFrom" v-loading="result.loading">

    <el-row>
      <el-col :span="8" :offset="1">
        <el-form-item
          :placeholder="$t('test_track.case.input_name')"
          :label="$t('test_track.case.name')"
          :label-width="formLabelWidth"
          prop="name">
          <el-input class="case-name" :disabled="readOnly" v-model="testCase.name"></el-input>
        </el-form-item>
      </el-col>

      <el-col :span="11" :offset="2">
        <el-form-item :label="$t('test_track.case.module')" :label-width="formLabelWidth" prop="module">
          <el-select
            v-model="testCase.module"
            :disabled="readOnly"
            :placeholder="$t('test_track.case.input_module')"
            filterable>
          </el-select>
        </el-form-item>
      </el-col>
    </el-row>

    <el-row>
      <el-col :span="10" :offset="1">
        <el-form-item :label="$t('test_track.case.maintainer')" :label-width="formLabelWidth" prop="maintainer">
          <el-select :disabled="readOnly" v-model="testCase.maintainer"
                     :placeholder="$t('test_track.case.input_maintainer')" filterable>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="12">
        <el-form-item :label="$t('test_track.case.priority')" :label-width="formLabelWidth" prop="priority">
          <el-select :disabled="readOnly" v-model="testCase.priority" clearable
                     :placeholder="$t('test_track.case.input_priority')">
          </el-select>
        </el-form-item>
      </el-col>
    </el-row>

    <el-row>
      <el-col :span="10" :offset="1">
        <el-form-item :label="$t('test_track.case.type')" :label-width="formLabelWidth" prop="type">
          <el-select :disabled="readOnly" v-model="testCase.type"
                     :placeholder="$t('test_track.case.input_type')">
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="12">
        <el-form-item :label="$t('test_track.case.method')" :label-width="formLabelWidth" prop="method">
          <el-select :disabled="readOnly" v-model="testCase.method" :placeholder="$t('test_track.case.input_method')">
          </el-select>
        </el-form-item>
      </el-col>
    </el-row>

    <el-row v-if="testCase.method && testCase.method == 'auto'">
      <el-col :span="9" :offset="1">
        <el-form-item :label="$t('test_track.case.relate_test')" :label-width="formLabelWidth" prop="testId">
          <el-select filterable :disabled="readOnly" v-model="testCase.testId"
                     :placeholder="$t('test_track.case.input_type')">
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="9" :offset="1" v-if="testCase.testId=='other'">
        <el-form-item :label="$t('test_track.case.test_name')" :label-width="formLabelWidth" prop="testId">
          <el-input v-model="testCase.otherTestName" :placeholder="$t('test_track.case.input_test_case')"></el-input>
        </el-form-item>
      </el-col>
    </el-row>
    <el-row style="margin-top: 15px;">
      <el-col :offset="2">{{ $t('test_track.case.prerequisite') }}:</el-col>
    </el-row>
    <el-row type="flex" justify="center" style="margin-top: 10px;">
      <el-col :span="20">
        <el-form-item prop="prerequisite">
          <el-input :disabled="readOnly" v-model="testCase.prerequisite"
                    type="textarea"
                    :autosize="{ minRows: 2, maxRows: 4}"
                    :rows="2"
                    :placeholder="$t('test_track.case.input_prerequisite')"></el-input>
        </el-form-item>
      </el-col>
    </el-row>

    <el-row v-if="testCase.method && testCase.method != 'auto'" style="margin-bottom: 10px">
      <el-col :offset="2">{{ $t('test_track.case.steps') }}:</el-col>
    </el-row>

    <el-row v-if="testCase.method && testCase.method != 'auto'" type="flex" justify="center">
      <el-col :span="20">
        <el-table
          v-if="isStepTableAlive"
          :data="JSON.parse(testCase.steps)"
          class="tb-edit"
          border
          size="mini"
          :default-sort="{prop: 'num', order: 'ascending'}"
          highlight-current-row>
          <el-table-column :label="$t('test_track.case.number')" prop="num" min-width="15%"></el-table-column>
          <el-table-column :label="$t('test_track.case.step_desc')" prop="desc" min-width="35%">
            <template v-slot:default="scope">
              <el-input
                class="table-edit-input"
                size="mini"
                :disabled="readOnly"
                type="textarea"
                :autosize="{ minRows: 1, maxRows: 6}"
                :rows="2"
                v-model="scope.row.desc"
                :placeholder="$t('commons.input_content')"
                clearable/>
            </template>
          </el-table-column>
          <el-table-column :label="$t('test_track.case.expected_results')" prop="result" min-width="35%">
            <template v-slot:default="scope">
              <el-input
                class="table-edit-input"
                size="mini"
                :disabled="readOnly"
                type="textarea"
                :autosize="{ minRows: 1, maxRows: 6}"
                :rows="2"
                v-model="scope.row.result"
                :placeholder="$t('commons.input_content')"
                clearable/>
            </template>
          </el-table-column>
        </el-table>
      </el-col>
    </el-row>

    <el-row style="margin-top: 15px;margin-bottom: 10px">
      <el-col :offset="2">{{ $t('commons.remark') }}:</el-col>
    </el-row>
    <el-row type="flex" justify="center">
      <el-col :span="20">
        <el-form-item prop="remark">
          <el-input v-model="testCase.remark"
                    :autosize="{ minRows: 2, maxRows: 4}"
                    type="textarea"
                    :disabled="readOnly"
                    :rows="2"
                    :placeholder="$t('commons.input_content')"></el-input>
        </el-form-item>
      </el-col>
    </el-row>
  </el-form>

</template>

<script>

export default {
  name: "TestCaseDetail",
  data() {
    return {
      result: {},
      dialogFormVisible: false,
      readOnly: true,
      form: {
        name: '',
        module: '',
        maintainer: '',
        priority: '',
        type: '',
        method: '',
        prerequisite: '',
        testId: '',
        otherTestName: '',
        steps: [{
          num: 1,
          desc: '',
          result: ''
        }],
        remark: '',
      },
      workspaceId: '',
      formLabelWidth: "120px",

      isStepTableAlive: true,
      methodOptions: [
        {value: 'auto', label: this.$t('test_track.case.auto')},
        {value: 'manual', label: this.$t('test_track.case.manual')}
      ]
    };
  },
  props: {
    testCase: {
      type: Object
    }
  },
}
</script>

<style scoped>

</style>
