<template>
  <div>
    <tw-opportunity-steps-summary-view
        :showData="this.shangJiZhaiYao">
    </tw-opportunity-steps-summary-view>

    <el-divider></el-divider>

    <div style="text-align:center; display: flex; justify-content: center; align-items: center;">
      <h3 style="flex: 1;">立项审批信息</h3>
      <h3 style="color: red;margin-right: 10%">{{ ApprovalStatus }}</h3>
    </div>

    <div style="width: 80%;margin: 0 auto">
      <el-form :model="liXiangShenPiInfo" :inline-message="true" :rules="rules" ref="submitForm" size="small"
               label-position="right" label-width="155px">
        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="立项名称" prop="projectName">
              <el-input v-model="liXiangShenPiInfo.projectName" auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="立项编号" prop="projectNumber">
              <el-input v-model="liXiangShenPiInfo.projectNumber" auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="招标机构名称" prop="biddingAgencyName">
              <el-input v-model="liXiangShenPiInfo.biddingAgencyName" auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="标书发布时间" prop="bidPublishTime">
              <el-date-picker
                  v-model="liXiangShenPiInfo.bidPublishTime"
                  type="datetime"
                  value-format="yyyy-MM-dd HH:mm:ss"
                  placeholder="选择日期时间"
                  align="right"
              >
              </el-date-picker>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="保证金金额（元）" prop="bidBondAmount">
              <el-input-number v-model="liXiangShenPiInfo.bidBondAmount" :min="0" :precision="2"
                               :step="0.01"></el-input-number>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="保证金截止时间" prop="bidBondDeadline">
              <el-date-picker
                  v-model="liXiangShenPiInfo.bidBondDeadline"
                  type="datetime"
                  value-format="yyyy-MM-dd HH:mm:ss"
                  placeholder="选择日期时间"
                  align="right"
              >
              </el-date-picker>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="拦标价（万元）" prop="bidTargetPrice">
              <el-input-number v-model="liXiangShenPiInfo.bidTargetPrice" :precision="2" :step="0.01"></el-input-number>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="人均费用限价(元/月/人)" prop="avgCostLimit">
              <el-input-number v-model="liXiangShenPiInfo.avgCostLimit" :precision="2" :step="0.01"></el-input-number>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="中小微企业价格扣除" prop="smesPriceDeduction">
              <el-select v-model="liXiangShenPiInfo.smesPriceDeduction" auto-complete="off" size="small"
                         placeholder="请选择">
                <el-option label="是" value="1"></el-option>
                <el-option label="否" value="0"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="合同期限（月）" prop="contractDurationMonths">
              <el-input-number v-model="liXiangShenPiInfo.contractDurationMonths" :min="1" :max="60" :step="1"
                               :controls="false" placeholder="请输入1-60数字">
              </el-input-number>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="投标截止时间" prop="bidDeadline">
              <el-date-picker
                  v-model="liXiangShenPiInfo.bidDeadline"
                  type="datetime"
                  value-format="yyyy-MM-dd HH:mm:ss"
                  placeholder="选择日期时间"
                  align="right"
              >
              </el-date-picker>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="准入类型标识" prop="accessTypeIdentifier">
              <el-select v-model="liXiangShenPiInfo.accessTypeIdentifier" placeholder="必选">
                <el-option v-for="(item,index) in this.$store.getters.access_type_identifier"
                           :key="index" :label="item.title" :value="item.title"/>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="24">
            <el-form-item label="项目经理要求" prop="projectManagerRequirements">
              <el-input v-model="liXiangShenPiInfo.projectManagerRequirements" auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="24">
            <el-form-item label="评分权重说明">
              <el-row :gutter="0">
                <el-col :span="6">
                  <el-form-item label-width="35%" label="资质分" prop="qualificationScore">
                    <el-input v-model="liXiangShenPiInfo.qualificationScore" type="number" auto-complete="off"
                              size="small"/>
                  </el-form-item>
                </el-col>
                <el-col :span="6">
                  <el-form-item label-width="35%" label="技术分" prop="technicalScore">
                    <el-input v-model="liXiangShenPiInfo.technicalScore" type="number" auto-complete="off"
                              size="small"/>
                  </el-form-item>
                </el-col>
                <el-col :span="6">
                  <el-form-item label-width="35%" label="商务分" prop="businessScore">
                    <el-input v-model="liXiangShenPiInfo.businessScore" type="number" auto-complete="off" size="small"/>
                  </el-form-item>
                </el-col>
                <el-col :span="6">
                  <el-form-item label-width="35%" label="价格分" prop="priceScore">
                    <el-input v-model="liXiangShenPiInfo.priceScore" type="number" auto-complete="off" size="small"/>
                  </el-form-item>
                </el-col>
              </el-row>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="24">
            <el-form-item label="评分加扣说明" prop="scoreAdjustmentNotes">
              <el-input v-model="liXiangShenPiInfo.scoreAdjustmentNotes" auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="24">
            <el-form-item label="预计投标单位" prop="expectedBidders">
              <el-input v-model="liXiangShenPiInfo.expectedBidders" auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="预计产值（万元/年）" prop="expectedOutputAnnual">
              <el-input-number v-model="liXiangShenPiInfo.expectedOutputAnnual" :precision="2"
                               :step="0.01"></el-input-number>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="预计成本（万元/年）" prop="expectedCostAnnual">
              <el-input-number v-model="liXiangShenPiInfo.expectedCostAnnual" :precision="2"
                               :step="0.01"></el-input-number>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="预计毛利润（万元/年）" prop="expectedGrossProfitAnnual">
              <el-input-number v-model="liXiangShenPiInfo.expectedGrossProfitAnnual" :precision="2"
                               :step="0.01"></el-input-number>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="预计利润率" prop="expectedProfitMargin">
              <el-select v-model="liXiangShenPiInfo.expectedProfitMargin" placeholder="必选">
                <el-option v-for="(item,index) in this.$store.getters.expected_profit_margin"
                           :key="index" :label="item.title" :value="item.title"/>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="预计员工总数" prop="expectedTotalEmployees">
              <el-input v-model="liXiangShenPiInfo.expectedTotalEmployees" oninput="value=value.replace(/[^\d]/g,'')"
                        type="number">
              </el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="拟派项目经理" prop="proposedProjectManager">
              <el-input v-model="liXiangShenPiInfo.proposedProjectManager" auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="12">
            <el-form-item label="发起人" prop="createUser">
              <el-input v-model="liXiangShenPiInfo.createUser" disabled auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="发起时间" prop="initiationTime">
              <el-input v-model="liXiangShenPiInfo.initiationTime" disabled auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="24">
            <el-form-item label="备注" prop="notes">
              <el-input v-model="liXiangShenPiInfo.notes" auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="24">
          <el-col :span="24">
            <el-form-item label="附件" prop="attachments">
              <el-input v-model="liXiangShenPiInfo.attachments" auto-complete="off" size="small"/>
            </el-form-item>
          </el-col>
        </el-row>

        <h3 style="text-align:center;">收入预测信息</h3>
        <div>
          <el-table
              :data="tableData"
              style="width: 100%"
          >
            <el-table-column prop="year" label="年度">
            </el-table-column>
            <el-table-column prop="date" label="总产值（万元）">
              <template v-slot="scope">
                <el-input type="number" v-model="tableData[scope.$index].totalOutput"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="date" label="总成本（万元）">
              <template v-slot="scope">
                <el-input type="number" v-model="tableData[scope.$index].totalCost"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="date" label="饱和收入（万元）">
              <template v-slot="scope">
                <el-input type="number" v-model="tableData[scope.$index].saturationRevenue"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="date" label="收缴率（%）">
              <template v-slot="scope">
                <el-input type="number" v-model="tableData[scope.$index].collectionRate"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="date" label="实际收入（万元）">
              <template v-slot="scope">
                <el-input type="number" v-model="tableData[scope.$index].actualRevenue"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="date" label="利润总额（万元）">
              <template v-slot="scope">
                <el-input type="number" v-model="tableData[scope.$index].totalProfit"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="date" label="毛利润（万元）">
              <template v-slot="scope">
                <el-input type="number" v-model="tableData[scope.$index].grossProfit"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="date" label="利润率（%）">
              <template v-slot="scope">
                <el-input type="number" v-model="tableData[scope.$index].profitMargin"></el-input>
              </template>
            </el-table-column>
          </el-table>
        </div>

        <h3 style="text-align:center;">所需资料信息</h3>
        <div>
          <el-table :data="liXiangShenPiInfo.tableResourceData"
                    ref="tableResource"
                    style="width: 100%">
            <el-table-column prop="operation" label="操作" width="50">
              <template v-slot="scope">
                <!-- 如果是最后一行，则显示添加按钮 -->
                <el-button type="primary" icon="el-icon-plus" @click="addRow"></el-button>
                <!-- 如果不是最后一行，则显示删除按钮 -->
                <el-button type="danger" icon="el-icon-delete"
                           @click="deleteRow(scope.$index)"></el-button>
              </template>
            </el-table-column>
            <el-table-column prop="responsiblePerson" label="责任人">
              <template v-slot="scope">
                <el-input v-model="liXiangShenPiInfo.tableResourceData[scope.$index].responsiblePerson"
                          @click.native="openuserDialog(scope.$index)"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="unitDepartment" label="单位部门">
              <template v-slot="scope">
                <el-input readonly
                          v-model="liXiangShenPiInfo.tableResourceData[scope.$index].unitDepartment"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="nameOfRequiredMaterials" label="所需材料名称">
              <template v-slot="scope">
                <el-form-item v-if="isEmpty(scope.$index)"
                              prop="'liXiangShenPiInfo.tableResourceData.' + scope.$index + '.nameOfRequiredMaterials'"
                              label-width="0" disabled
                              :rules="rules.nameOfRequiredMaterials">
                  <el-input
                      v-model="liXiangShenPiInfo.tableResourceData[scope.$index].nameOfRequiredMaterials"></el-input>
                </el-form-item>
                <el-form-item v-else label-width="0">
                  <el-input
                      v-model="liXiangShenPiInfo.tableResourceData[scope.$index].nameOfRequiredMaterials"></el-input>
                </el-form-item>
              </template>
            </el-table-column>
            <el-table-column prop="requestForTime" label="要求提供时间">
              <template v-slot="scope">
                <el-date-picker
                    v-model="liXiangShenPiInfo.tableResourceData[scope.$index].requestForTime"
                    type="datetime"
                    value-format="yyyy-MM-dd HH:mm:ss"
                    placeholder="选择日期时间"
                    align="right"
                >
                </el-date-picker>
              </template>
            </el-table-column>
            <el-table-column prop="remarks" label="备注">
              <template v-slot="scope">
                <el-input v-model="liXiangShenPiInfo.tableResourceData[scope.$index].remarks"></el-input>
              </template>
            </el-table-column>
          </el-table>
        </div>

        <div style="text-align: center;margin-top: 20px">
          <el-button size="small" type="primary" @click="baocun">保存</el-button>
          <el-button size="small" type="primary" @click="songshen">送审</el-button>
          <el-button size="small" type="primary" @click="fanhui">返回</el-button>
        </div>

        <tw-opportunity-select-user :visible.sync="openUser" @selected="selectuser"></tw-opportunity-select-user>

      </el-form>
    </div>


  </div>
</template>

<script>


export default {
  components: {},
  props: {
    id: {
      type: Number,
      default: null,
    },
    row_obj: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      active: 2,
      rules: {
        projectName: [
          {required: true, message: '请输入立项名称', trigger: 'blur'},
        ],
        biddingAgencyName: [
          {required: true, message: '请输入招标机构名称', trigger: 'blur'},
        ],
        bidPublishTime: [
          {required: true, message: '请输入标书发布时间', trigger: 'blur'},
        ],
        bidBondAmount: [
          {required: true, message: '请输入保证金金额', trigger: 'blur'},
        ],
        smesPriceDeduction: [
          {required: true, message: '请选择', trigger: 'blur'},
        ],
        contractDurationMonths: [
          {required: true, message: '请输入合同期限', trigger: 'blur'},
        ],
        bidDeadline: [
          {required: true, message: '请输入投标截止时间', trigger: 'blur'},
        ],
        accessTypeIdentifier: [
          {required: true, message: '请选择', trigger: 'blur'},
        ],
        expectedProfitMargin: [
          {required: true, message: '请输入预计利润率', trigger: 'blur'},
        ],

        //表格验证
        nameOfRequiredMaterials: [
          {required: true, message: '请输入所需材料名称', trigger: 'blur'}
        ]
      },
      //审批状态
      ApprovalStatus: "未提交",

      tableData: [
        {
          year: '第一年',
          totalOutput: null,
          totalCost: null,
          saturationRevenue: null,
          collectionRate: null,
          actualRevenue: null,
          totalProfit: null,
          grossProfit: null,
          profitMargin: null,
        },
        {
          year: '第二年',
          totalOutput: null,
          totalCost: null,
          saturationRevenue: null,
          collectionRate: null,
          actualRevenue: null,
          totalProfit: null,
          grossProfit: null,
          profitMargin: null,
        },
        {
          year: '第三年',
          totalOutput: null,
          totalCost: null,
          saturationRevenue: null,
          collectionRate: null,
          actualRevenue: null,
          totalProfit: null,
          grossProfit: null,
          profitMargin: null,
        }
      ],
      //商机摘要
      shangJiZhaiYao: {
        //商机名称
        shangJiName: null,
        //商机状态
        shangJiType: null,
        //客户名称
        customerName: null,
        //客户等级
        customerDenJi: null,
        //项目名称
        commName: null,
        //项目业态
        commYeTai: null,
        //是否新项目
        isNewComm: null,
        //管理范围
        guanLiLimit: null,
        //所属单位
        suoShuDanWei: null,
        //创建人
        createUser: null,
        //创建时间
        createDate: null,
        //跟进人
        gengJinUser: null
      },
      //立项审批信息
      liXiangShenPiInfo: {
        projectName: null,
        projectNumber: null,
        biddingAgencyName: null,
        bidPublishTime: null,
        bidBondAmount: null,
        bidBondDeadline: null,
        bidTargetPrice: null,
        avgCostLimit: null,
        smesPriceDeduction: null,
        contractDurationMonths: null,
        bidDeadline: null,
        accessTypeIdentifier: null,
        projectManagerRequirements: null,
        qualificationScore: null,
        technicalScore: null,
        businessScore: null,
        priceScore: null,
        scoreAdjustmentNotes: null,
        expectedBidders: null,
        expectedOutputAnnual: null,
        expectedCostAnnual: null,
        expectedGrossProfitAnnual: null,
        expectedProfitMargin: null,
        expectedTotalEmployees: null,
        proposedProjectManager: null,
        createUser: null,
        initiationTime: null,
        notes: null,
        attachments: null,
        tableResourceData: [],
      },
      openUser: false,
      hang: null,

    }
  },
  created() {
    this.$nextTick(() => {
      // 如果表格数据为空，则添加一个空对象
      if (this.liXiangShenPiInfo.tableResourceData.length === 0) {
        this.addRow();
      }
    });

  },
  methods: {
    submitForm() {
      let type = false;
      this.$refs['submitForm'].validate((valid) => {
        if (valid) {
          type = true;
        } else {
          console.log('error submit!!');
          type = false;
        }
      });
      return type;
    },
    isEmpty(index) {
      return this.liXiangShenPiInfo.tableResourceData[index].id !== null;
    },
    openuserDialog(val) {
      this.hang = val;
      this.openUser = true;
    },
    baocun() {
      if (this.submitForm()) {
        console.log("保存")
      } else {
        this.$message.error("请填写完整信息")
      }

    },
    songshen() {
      if (this.submitForm()) {
        console.log("送审")
      } else {
        this.$message.error("请填写完整信息")
      }
    },
    fanhui() {
      console.log("返回")
    },
    selectuser(data) {
      let index = this.hang;
      console.log(data)
      this.liXiangShenPiInfo.tableResourceData[index].id = data.data[0].id;
      this.liXiangShenPiInfo.tableResourceData[index].responsiblePerson = data.data[0].name;
      this.liXiangShenPiInfo.tableResourceData[index].unitDepartment = data.data[0].unitDepartment;

      console.log(data)
    },
    // 所需资料信息添加一行
    addRow() {
      // 创建一个空对象，用来存储新行的数据
      let newRow = {
        id: null,
        responsiblePerson: null,
        unitDepartment: null,
        nameOfRequiredMaterials: null,
        requestForTime: null,
        remarks: null,
      };
      this.liXiangShenPiInfo.tableResourceData.push(newRow);
    },
    // 所需资料信息删除一行
    deleteRow(index) {
      this.$confirm('此操作将在当前页面删除该行数据, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // 从表格数据的数组中移除指定索引的元素
        this.liXiangShenPiInfo.tableResourceData.splice(index, 1);
        // 判断表格数据是否为空
        if (this.liXiangShenPiInfo.tableResourceData.length === 0) {
          // 如果是，则添加一个空对象
          this.addRow();
        }
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });

    },

  },
  computed: {}

}
</script>

<style scoped>

</style>
