<template>
  <div class="common-layout">
    <el-container>
      <el-header height="65px" class="container">
        <el-row align="middle" class="header">
          <el-col :span="12">
            <el-col>
              <el-text type="warning">회원관리</el-text>
            </el-col>
            <el-col>
              <el-text class="title">회원/CRM 관리하기</el-text>
            </el-col>
          </el-col>
          <el-col :span="12" class="right">
            <el-button
              type="info"
              plain
              class="banner"
              size="large"
            >
              <span class="banner-text">배너영역</span>
            </el-button>
          </el-col>
        </el-row>
      </el-header>
      <el-main class="container">
        <el-col class="main">
          <el-row>
            <el-text size="small">회원 및 추천인에 관련된 모든 정보를 한페이지에서 관리하실 수 있습니다.</el-text>
            <el-button
                type="info"
                size="small"
                :icon="ElIconMemo"
                class="btn-menu__info"
            >
              <span>메뉴설명</span>
            </el-button>
          </el-row>
          <el-col>
            <el-form
                class="form"
                label-width="120px"
                label-position="right"
            >
              <el-form-item label="회원검색">
                <el-input
                    v-model="userInput"
                    placeholder="검색어를 입력해주세요"
                    class="input-with-select"
                    clearable
                    style="width: 480px"
                >
                  <template #prepend>
                    <el-select v-model="userSelect" placeholder="검색조건" style="width: 180px">
                      <el-option label="회원명" value="name" />
                      <el-option label="이메일" value="email" />
                      <el-option label="연락처" value="phone" />
                      <el-option label="휴대폰번호" value="mobile" />
                      <el-option label="생년월일" value="birth" />
                      <el-option label="CID" value="CID" />
                    </el-select>
                  </template>
                </el-input>
              </el-form-item>
              <el-form-item label="그룹검색">
                <el-input
                    v-model="groupInput"
                    placeholder="그룹 내 회원명을 입력해주세요"
                    class="input-with-select"
                    clearable
                    style="width: 480px"
                >
                  <template #prepend>
                    <el-select v-model="groupSelect" placeholder="그룹선택" style="width: 180px">
                      <el-option label="그룹1" value="group1" />
                    </el-select>
                  </template>
                </el-input>
              </el-form-item>
              <el-form-item label="CRM 그룹검색">
                <el-radio-group v-model="crmRadio">
                  <el-radio label="all">전체회원</el-radio>
                  <el-radio label="best">최우수고객</el-radio>
                  <el-radio label="better">우수고객</el-radio>
                  <el-radio label="normal">일반고객</el-radio>
                  <el-radio label="warning">주의고객</el-radio>
                  <el-radio label="complain">항의고객</el-radio>
                </el-radio-group>
              </el-form-item>
              <el-form-item label="조건/날짜 검색">
                <client-only>
                  <el-select
                      v-model="caseSelect"
                      style="width: 180px; margin-right: 10px"
                  >
                    <el-option label="로그인" value="login" />
                    <el-option label="회원가입" value="join" />
                    <el-option label="휴면예정 회원" value="dormant" />
                    <el-option label="생일" value="birth" />
                    <el-option label="추천인" value="recommend" />
                  </el-select>
                  <div class="demo-date-picker">
                    <div class="block" v-show="caseSelect !== 'birth'">
                      <el-date-picker
                        v-model="datePicker"
                        type="daterange"
                        unlink-panels
                        range-separator="~"
                        start-placeholder="시작날짜 선택"
                        end-placeholder="종료날짜 선택"
                        :shortcuts="shortcuts"
                        :disabledDate="disabledDate"
                        style="width: 270px; margin-top: 1px"
                      />
                    </div>
                    <div class="block" v-show="caseSelect === 'birth'">
                      <el-date-picker
                        v-model="birthPicker"
                        type="date"
                        placeholder="날짜 선택"
                        style="width: 150px; margin-bottom: 2px"
                      />
                    </div>
                  </div>
                </client-only>
                <el-input
                    v-show="caseSelect === 'recommend'"
                    v-model="recommendId"
                    placeholder="추천인ID"
                    clearable
                    style="width: 150px; margin-left: 10px"
                />
              </el-form-item>
              <el-form-item
                  label="가입경로"
                  v-show="caseSelect === 'join'"
                  class="sub-label"
              >
                <client-only>
                  <el-select
                      v-model="joinRootSelect"
                      style="width: 180px"
                  >
                    <el-option label="전체" value="all" />
                    <el-option label="WEB" value="web" />
                    <el-option label="MOBILE" value="mobile" />
                  </el-select>
                </client-only>
              </el-form-item>
              <el-form-item
                  label="가입형태"
                  v-show="caseSelect === 'join'"
                  class="sub-label"
              >
                <client-only>
                  <el-select
                      v-model="joinTypeSelect"
                      style="width: 180px"
                  >
                    <el-option label="전체" value="all" />
                    <el-option label="일반" value="normal" />
                    <el-option label="간편" value="easy" />
                    <el-option label="간편(SNS)" value="sns" />
                    <el-option label="다이렉트RM" value="direct" />
                  </el-select>
                </client-only>
              </el-form-item>
              <el-form-item
                  label=""
                  v-show="caseSelect === 'recommend'"
              >
                <el-alert
                    type="info"
                    show-icon
                    :closable="false"
                >
                  <p>추천인 ID를 입력하지 않으면, 추천인 중 상위 10명을 검색합니다.</p>
                </el-alert>
              </el-form-item>
              <el-form-item label="파워앱 관련회원">
                <el-input
                    v-model="powerAppInput"
                    placeholder="앱에서 초대한 아이디를 입력해주세요."
                    class="input-with-select"
                    clearable
                    style="width: 480px"
                >
                  <template #prepend>
                    <el-select v-model="powerAppSelect" style="width: 180px">
                      <el-option label="전체회원(인증+삭제)" value="all" />
                      <el-option label="인증회원" value="authorized" />
                      <el-option label="삭제회원" value="deleted" />
                    </el-select>
                  </template>
                </el-input>
              </el-form-item>
              <el-form-item
                  label="타겟"
                  v-show="powerAppSelect === 'authorized'"
                  class="sub-label"
              >
                <client-only>
                  <el-tooltip class="box-item" effect="dark">
                    <template #content>
                      회원(인증상태): 앱에 로그인 중이거나, 앱을 삭제하지 않은 회원<br>
                      회원(인증해제 상태): 앱에 해당 아이디로 로그인 후, 다른 아이디로 로그인한 회원
                    </template>
                    <el-select
                        v-model="authorizedSelect"
                        style="width: 180px"
                    >
                      <el-option label="인증회원 전체" value="all" />
                      <el-option label="회원(인증상태)" value="on" />
                      <el-option label="회원(인증해제 상태)" value="off" />
                    </el-select>
                  </el-tooltip>
                </client-only>
              </el-form-item>
              <el-form-item
                  label="푸시알림"
                  v-show="powerAppSelect === 'authorized'"
                  class="sub-label"
              >
                <client-only>
                  <el-select
                      v-model="pushSelect"
                      style="width: 180px;"
                  >
                    <el-option label="전체" value="all" />
                    <el-option label="푸시ON" value="on" />
                    <el-option label="푸시OFF" value="off" />
                  </el-select>
                </client-only>
              </el-form-item>
              <el-form-item label="">
                <el-alert
                    type="info"
                    show-icon
                    :closable="false"
                >
                  <p>
                    삭제회원은 실시간이 아니며 푸시를 발송한 날 기준으로 익일 새벽에 업데이트 됩니다.
                  </p>
                </el-alert>
              </el-form-item>
              <el-form-item style="display:flex; align-items: center; flex-direction: column; margin-right: 190px">
                <el-button type="primary" size="large" @click="onSubmit">검색하기</el-button>
                <el-button size="large">초기화</el-button>
              </el-form-item>
            </el-form>
          </el-col>
          <el-row justify="end">
            <client-only>
              <el-dropdown>
                <el-button type="primary">
                  관리메뉴 더보기
                  <el-icon class="el-icon--right"><arrow-down /></el-icon>
                </el-button>
                <template #dropdown>
                  <el-dropdown-menu>
                    <el-dropdown-item>적립금 수기지급 조회</el-dropdown-item>
                    <el-dropdown-item>적립금 지급요청 관리</el-dropdown-item>
                    <el-dropdown-item divided>
                      평생
                      <el-icon class="el-icon--right"><right /></el-icon>
                      일반 전환내역
                    </el-dropdown-item>
                    <el-dropdown-item>
                      휴면
                      <el-icon class="el-icon--right"><right /></el-icon>
                      일반 전환내역
                    </el-dropdown-item>
                    <el-dropdown-item divided>부정의심 로그인</el-dropdown-item>
                    <el-dropdown-item>회원수기 등록</el-dropdown-item>
                    <el-dropdown-item> 회원등급 관리</el-dropdown-item>
                    <el-dropdown-item> 회원탈퇴 관리</el-dropdown-item>
                  </el-dropdown-menu>
                </template>
              </el-dropdown>
            </client-only>
          </el-row>
          <el-row>
            <el-button
                type="info"
                size="small"
                style="margin-top: 10px"
            >
              <span>엑셀 다운로드</span>
            </el-button>
          </el-row>
          <el-col>
            <client-only>
              <el-table
                :data="tableData"
                border
                stripe
                height="400"
                style="width: 100%; margin-top: 10px"
              >
                <el-table-column type="selection" width="55" />
                <el-table-column prop="ip" label="IP" width="55"  />
                <el-table-column prop="date" label="등록일" />
                <el-table-column prop="id" label="아이디" />
                <el-table-column prop="name" label="성명" />
                <el-table-column label="주소" width="100">
                  <el-row>
                    <house class="icon-in-table"/>
                    <office-building class="icon-in-table" style="margin-top: 2px;"/>
                  </el-row>
                </el-table-column>
                <el-table-column label="연락처" width="100">
                  <el-row>
                    <phone class="icon-in-table"/>
                    <cellphone class="icon-in-table"/>
                  </el-row>
                </el-table-column>
                <el-table-column prop="email" label="이메일" />
                <el-table-column label="메모" width="55">
                  <el-row>
                    <memo class="icon-in-table"/>
                  </el-row>
                </el-table-column>
              </el-table>
            </client-only>
          </el-col>
        </el-col>
      </el-main>
    </el-container>
  </div>
</template>

<script setup lang="ts">
import { Right, ArrowDown, Memo, House, OfficeBuilding, Phone, Cellphone } from "@element-plus/icons-vue";

const userSelect = ref("");
const groupSelect = ref("");
const caseSelect = ref("login");
const joinRootSelect = ref("all");
const joinTypeSelect = ref("all");
const powerAppSelect = ref("all");
const authorizedSelect = ref("all");
const pushSelect = ref("all");
const userInput = ref("");
const groupInput = ref("");
const powerAppInput = ref("");
const recommendId = ref("");
const crmRadio = ref("all");
const datePicker = ref([new Date(), new Date()])
const birthPicker = ref(new Date())
const info = ref("")

const shortcuts = [
  {
    text: '오늘',
    value: [new Date(), new Date()],
  },
  {
    text: '어제',
    value: () => {
      const end = new Date()
      const start = new Date()
      start.setTime(start.getTime() - 3600 * 1000 * 24)
      return [start, end]
    },
  },
  {
    text: '3일전',
    value: () => {
      const end = new Date()
      const start = new Date()
      start.setTime(start.getTime() - 3600 * 1000 * 72)
      return [start, end]
    },
  },
  {
    text: '일주일 전',
    value: () => {
      const end = new Date()
      const start = new Date()
      start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
      return [start, end]
    },
  },
  {
    text: '한달 전',
    value: () => {
      const end = new Date()
      const start = new Date()
      start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
      return [start, end]
    },
  },
]

const tableData = [
  {
    ip: 1,
    date: "2023-02-01",
    id: "a11",
    name: "테스트",
    email: "abc@abcde.fg",
    address: "abcde",
    company: "xyz",
    phone: "032-123-4567",
    mobile: "010-1234-5678",
    reserve: 0,
    recommender: "recommender",
  },
  {
    ip: 2,
    date: "2023-01-01",
    id: "b22",
    name: "테스트2",
    email: "bbb@abcde.fg",
    address: "address",
    company: "company",
    phone: "02-123-4567",
    mobile: "010-0987-6543",
    reserve: 100,
    recommender: "recommender2",
  },
  {
    ip: 1,
    date: "2023-02-01",
    id: "a11",
    name: "테스트",
    email: "abc@abcde.fg",
    address: "abcde",
    company: "xyz",
    phone: "032-123-4567",
    mobile: "010-1234-5678",
    reserve: 0,
    recommender: "recommender",
  },
  {
    ip: 1,
    date: "2023-02-01",
    id: "a11",
    name: "테스트",
    email: "abc@abcde.fg",
    address: "abcde",
    company: "xyz",
    phone: "032-123-4567",
    mobile: "010-1234-5678",
    reserve: 0,
    recommender: "recommender",
  },
  {
    ip: 1,
    date: "2023-02-01",
    id: "a11",
    name: "테스트",
    email: "abc@abcde.fg",
    address: "abcde",
    company: "xyz",
    phone: "032-123-4567",
    mobile: "010-1234-5678",
    reserve: 0,
    recommender: "recommender",
  },
  {
    ip: 1,
    date: "2023-02-01",
    id: "a11",
    name: "테스트",
    email: "abc@abcde.fg",
    address: "abcde",
    company: "xyz",
    phone: "032-123-4567",
    mobile: "010-1234-5678",
    reserve: 0,
    recommender: "recommender",
  },
  {
    ip: 1,
    date: "2023-02-01",
    id: "a11",
    name: "테스트",
    email: "abc@abcde.fg",
    address: "abcde",
    company: "xyz",
    phone: "032-123-4567",
    mobile: "010-1234-5678",
    reserve: 0,
    recommender: "recommender",
  },
]

const disabledDate = (time: Date) => {
  return time.getTime() > Date.now()
}
</script>

<style scoped>
.common-layout {
  width: 100%;
  max-width: 1024px;
  background-color: #fff;
}
.container {
  margin: 0;
  padding: 0;
  height: 100%;
}
.header {
  border-bottom: 1px solid #e6e6e6;
  box-sizing: border-box;
  padding: 10px;
}
.title {
  font-size: 18px;
  font-weight: 700;
}
.right {
  text-align: right;
}
.banner {
  width: 40%;
}
.banner-text {
  font-size: small
}
.main {
  padding: 10px;
  box-sizing: border-box;
}
.btn-menu__info {
  margin-left: 5px;
}
.form {
  margin: 10px 0;
  padding: 15px 20px;
  box-sizing: border-box;
  border: 1px solid #e6e6e6;
  border-radius: 15px;
}
.sub-label {
  display: inline-flex;
}
.icon-in-table {
  width: 20px;
}
.icon-in-table:not(:first-child) {
  margin-left: 10px;
}
</style>