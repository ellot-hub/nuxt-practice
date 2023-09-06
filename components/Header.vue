<template>
  <el-row class="container" justify="space-between">
    <el-row align="middle">
      <el-breadcrumb separator=">" class="bread-crumb">
        <el-breadcrumb-item :to="{ path: '/' }">회원관리</el-breadcrumb-item>
        <el-breadcrumb-item>
          <a href="/">회원/CRM 관리하기</a>
        </el-breadcrumb-item>
      </el-breadcrumb>
      <el-button
          type="warning"
          size="small"
          :icon="ElIconStar"
          plain
          class="btn-setting"
      >
        <span>설정</span>
      </el-button>
      <el-button
          type="info"
          size="small"
          :icon="ElIconHelp"
          plain
          class="btn-setting"
      >
        <span>HELP</span>
      </el-button>
      <el-popover
          :visible="quickSearch"
          placement="bottom"
          :width="350"
      >
        <template #reference>
          <el-button
              type="primary"
              size="small"
              :icon="ElIconSearch"
              plain
              class="btn-setting"
              @click="openSearch = !openSearch"
          >
            <span>빠른 검색</span>
          </el-button>
        </template>
        <template #default>
          <el-text size="large" type="primary" tag="b">빠른 검색</el-text>
          <br>
          <el-radio-group
              v-model="searchRadio"
              class="quick-search"
          >
            <el-radio label="product">상품</el-radio>
            <el-radio label="user">회원</el-radio>
            <el-radio label="order">주문</el-radio>
          </el-radio-group>
          <el-input
              v-model="searchInput"
              class="input-with-select"
              clearable
          >
            <template #prepend>
              <el-select
                v-model="rangeSelect"
                style="width: 100px"
              >
                <div v-for="list in searchList.product">
                  <el-option :label="list.label" :value="list.value" />
                </div>
              </el-select>
            </template>
            <template #append>
              <el-button :icon="ElIconSearch"/>
            </template>
          </el-input>
        </template>
      </el-popover>
    </el-row>
    <el-row align="middle">
      <el-space>
        <client-only>
          <el-dropdown>
            <span class="el-dropdown-link">
              <el-text class="dropdown-text">내 쇼핑몰</el-text>
              <el-icon class="el-icon--right">
                <caret-bottom />
              </el-icon>
            </span>
            <template #dropdown>
              <el-dropdown-menu>
                <el-dropdown-item>PC 쇼핑몰</el-dropdown-item>
                <el-dropdown-item>MOBILE 쇼핑몰</el-dropdown-item>
                <el-dropdown-item>글로비 로그인</el-dropdown-item>
                <el-dropdown-item>마이페이지</el-dropdown-item>
                <el-dropdown-item divided>서비스 이용현황</el-dropdown-item>
                <el-dropdown-item>쇼핑몰 운영 캘린더</el-dropdown-item>
                <el-dropdown-item>트래픽 조회</el-dropdown-item>
                <el-dropdown-item divided>쇼핑몰 로그 조회</el-dropdown-item>
                <el-dropdown-item>로그인 본인인증 확인</el-dropdown-item>
                <el-dropdown-item>비밀번호 변경</el-dropdown-item>
                <el-dropdown-item>카카오 상담톡</el-dropdown-item>
              </el-dropdown-menu>
            </template>
          </el-dropdown>
        </client-only>
        <client-only>
          <el-dropdown>
            <span class="el-dropdown-link">
              <el-text class="dropdown-text">고객센터</el-text>
              <el-icon class="el-icon--right">
                <caret-bottom />
              </el-icon>
            </span>
            <template #dropdown>
              <el-dropdown-menu>
                <el-dropdown-item>자주 묻는 질문</el-dropdown-item>
                <el-dropdown-item>1:1 문의</el-dropdown-item>
                <el-dropdown-item>톡상담</el-dropdown-item>
                <el-dropdown-item>처리 동의서</el-dropdown-item>
                <el-dropdown-item>기타비용 결제</el-dropdown-item>
                <el-dropdown-item divided>전자상거래 법률 소식</el-dropdown-item>
                <el-dropdown-item>회원탈퇴 신청</el-dropdown-item>
              </el-dropdown-menu>
            </template>
          </el-dropdown>
        </client-only>
        <el-input
          v-model="input"
          style="height: 24px"
          class="input-with-select"
        >
          <template #append>
            <el-button :icon="ElIconSearch" />
          </template>
        </el-input>
      </el-space>
    </el-row>
  </el-row>
</template>

<script setup lang="ts">
import { CaretBottom } from "@element-plus/icons-vue";
import {reactive, computed} from "@vue/reactivity";

const searchList = {
  product: [
    {
      label: "전체상품",
      value: "all"
    },
    {
      label: "키워드",
      value: "keyword"
    },
    {
      label: "메인추천 상품",
      value: "mainRecommend"
    },
    {
      label: "메인 신상품",
      value: "mainNew"
    },
    {
      label: "메인 특별상품",
      value: "mainSpecial"
    },
    {
      label: "대분류 추천상품",
      value: "mainCategoryRecommend"
    },
    {
      label: "메인 추가상품",
      value: "mainAdd"
    },
    {
      label: "품절된 상품",
      value: "soldout"
    },
    {
      label: "현금결제 상품",
      value: "cash"
    },
    {
      label: "상품 바코드",
      value: "barcode"
    },
    {
      label: "미진열 상품",
      value: "notExhibited"
    },
    {
      label: "상품코드",
      value: "code"
    },
    {
      label: "상품번호",
      value: "number"
    },
    {
      label: "스타일코드",
      value: "styleCode"
    },
    {
      label: "사입업체",
      value: "buyCompany"
    },
    {
      label: "추가 상품명",
      value: "addName"
    },
    {
      label: "영문 상품명",
      value: "engName"
    },
  ],
  user: [
    {
      label: "이름",
      value: "name"
    },
    {
      label: "ID",
      value: "id"
    },
    {
      label: "핸드폰",
      value: "mobile"
    }
  ],
  order: [
    {
      label: "주문번호",
      value: "number"
    },
    {
      label: "주문자명",
      value: "name"
    }
  ]
}

const input = ref("");
const searchInput = ref("");
const searchRadio = ref("product");
const openSearch = ref(false);
const rangeSelect = ref(searchList.product[0]);


</script>

<style scoped>
.container {
  margin: 0;
  height: 100%;
  padding: 10px 0;
  box-sizing: border-box;
}
.bread-crumb {
  font-size: 13px;
}
.btn-setting {
  margin-left: 10px;
}
.dropdown-text {
  font-size: 13px;
  vertical-align: top;
}
.quick-search {
  font-size: 14px;
}
</style>
