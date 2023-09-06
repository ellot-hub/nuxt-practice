<template>
  <div class="common-layout">
    <el-container>
      <el-header height="44px" class="header-fixed header-line">
        <Header/>
      </el-header>
      <el-container>
        <el-aside width="250px" class="aside-area">
          <el-scrollbar height="calc(100vh - 44px)">
            <client-only>
              <el-menu
                  default-active="1-1"
                  :default-openeds="['1-1','1-2','1-3','1-4']"
                  class="aside-menu"
                  @open="handleOpen"
                  @close="handleClose"
                  style="font-size: 14px;"
              >
                <el-sub-menu index="1">
                  <template #title>
                    <el-icon><user/></el-icon>
                    <span>회원관리</span>
                  </template>
                  <el-menu-item index="1-1">회원/CRM 관리하기</el-menu-item>
                  <el-menu-item index="1-2">휴면고객 관리</el-menu-item>
                  <el-menu-item index="1-3">개인정보 유효기간 혜택 설정</el-menu-item>
                  <el-menu-item index="1-4">회원 엑셀파일 관리</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="2">
                  <template #title>
                    <el-icon><gold-medal/></el-icon>
                    <span>회원등급별 쇼핑몰 설정</span>
                  </template>
                  <el-menu-item index="2-1">회원그룹 신규 생성/삭제</el-menu-item>
                  <el-menu-item index="2-2">회원그룹 부가기능</el-menu-item>
                  <el-menu-item index="2-3">그룹별 회원등록 관리</el-menu-item>
                  <el-menu-item index="2-4">회원 CSV 일괄 그룹등록</el-menu-item>
                  <el-menu-item index="2-5">그룹별 전체회원 관리</el-menu-item>
                  <el-menu-item index="2-6">그룹별 자동 등록 설정</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="3">
                  <template #title>
                    <el-icon><promotion/></el-icon>
                    <span>회원관리 프로모션</span>
                  </template>
                  <el-menu-item-group title="내역조회">
                    <el-menu-item index="3-1">예치금 내역조회</el-menu-item>
                    <el-menu-item index="3-2">적립금 내역조회</el-menu-item>
                    <el-menu-item index="3-3">포인트 내역조회</el-menu-item>
                  </el-menu-item-group>
                  <el-menu-item-group title="CSV 일괄지급">
                    <el-menu-item index="3-4">적립금 CSV 일괄지급</el-menu-item>
                    <el-menu-item index="3-5">예치금 CSV 일괄지급</el-menu-item>
                    <el-menu-item index="3-6">포인트 CSV 일괄지급</el-menu-item>
                  </el-menu-item-group>
                  <el-menu-item-group title="소멸관리">
                    <el-menu-item index="3-7">미로그인 회원 적립금 소멸</el-menu-item>
                    <el-menu-item index="3-8">미사용 적립금 소멸관리</el-menu-item>
                  </el-menu-item-group>
                  <el-menu-item-group title="지급관리">
                    <el-menu-item index="3-9">적립금/쿠폰 수기 지급 조회</el-menu-item>
                    <el-menu-item index="3-10">적립금 지급요청 관리</el-menu-item>
                    <el-menu-item index="3-11">예치금 지급요청 관리</el-menu-item>
                  </el-menu-item-group>
                  <el-menu-item-group title="회원관리">
                    <el-menu-item index="3-12">쇼핑몰 회원탈퇴 관리</el-menu-item>
                    <el-menu-item index="3-13">회원 일괄 인증 관리</el-menu-item>
                    <el-menu-item index="3-14">생일/기념일 축하 이벤트</el-menu-item>
                    <el-menu-item index="3-15">회원정보 수정 이벤트</el-menu-item>
                    <el-menu-item index="3-16">메일/SMS 엑셀 수신거부</el-menu-item>
                    <el-menu-item index="3-17">회원형태 일괄수정</el-menu-item>
                  </el-menu-item-group>
                </el-sub-menu>
                <el-sub-menu index="4">
                  <template #title>
                    <el-icon><coin/></el-icon>
                    <span>스마트 적립금</span>
                  </template>
                  <el-menu-item index="4-1">스마트 적립금 설정</el-menu-item>
                  <el-menu-item index="4-2">적립금 관리</el-menu-item>
                  <el-menu-item index="4-3">적립금 지급설정</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="5">
                  <template #title>
                    <el-icon><message-box/></el-icon>
                    <span>회원 쪽지보내기</span>
                  </template>
                  <el-menu-item index="5-1">회원 쪽지보내기 소개</el-menu-item>
                  <el-menu-item index="5-2">회원 쪽지내역 관리</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="6">
                  <template #title>
                    <el-icon><calendar/></el-icon>
                    <span>출석체크 서비스</span>
                  </template>
                  <el-menu-item index="6-1">출석체크 기능소개</el-menu-item>
                  <el-menu-item index="6-2">출석체크 기능설정</el-menu-item>
                  <el-menu-item index="6-3">출석체크 디자인 설정</el-menu-item>
                  <el-menu-item index="6-4">출석체크 리스트 관리</el-menu-item>
                  <el-menu-item index="6-5">출석체크 예약설정</el-menu-item>
                  <el-menu-item index="6-6">출석체크 예약/마감 내역</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="7">
                  <template #title>
                    <el-icon><shopping-cart/></el-icon>
                    <span>카트타겟 메세지(CTM)</span>
                  </template>
                  <el-menu-item index="7-1">카트타겟 메세지(CTM) 기능소개</el-menu-item>
                  <el-menu-item index="7-2">카트타겟 메세지(CTM) 발송</el-menu-item>
                  <el-menu-item index="7-3">카트타겟 메세지(CTM) 내역</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="8">
                  <template #title>
                    <el-icon><message/></el-icon>
                    <span>문자(SMS) 발송/관리</span>
                  </template>
                  <el-menu-item index="8-1">SMS 소개</el-menu-item>
                  <el-menu-item index="8-2">SMS 충전</el-menu-item>
                  <el-menu-item index="8-3">SMS 서비스설정</el-menu-item>
                  <el-menu-item index="8-4">SMS 보내기</el-menu-item>
                  <el-menu-item index="8-5">SMS 발송 메세지 관리</el-menu-item>
                  <el-menu-item index="8-6">SMS 발송내역 관리</el-menu-item>
                  <el-menu-item index="8-7">SMS 예약내역 관리</el-menu-item>
                  <el-menu-item index="8-8">주문회원 일괄 SMS 발송</el-menu-item>
                  <el-menu-item index="8-9">매출통계 SMS 발송</el-menu-item>
                  <el-menu-item index="8-10">SMS 주소록 간단관리</el-menu-item>
                  <el-menu-item index="8-11">회원전용 SMS 지급</el-menu-item>
                  <el-menu-item index="8-12">회원전용 SMS 지급내역</el-menu-item>
                  <el-menu-item index="8-13">SMS 정산</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="9">
                  <template #title>
                    <el-icon><chat-dot-round/></el-icon>
                    <span>카카오 알림톡</span>
                  </template>
                  <el-menu-item index="9-1">알림톡 소개</el-menu-item>
                  <el-menu-item index="9-2">알림톡 설정</el-menu-item>
                  <el-menu-item index="9-3">알림톡 템플릿 관리</el-menu-item>
                  <el-menu-item index="9-4">알림톡 템플릿 승인내역</el-menu-item>
                  <el-menu-item index="9-5">알림톡 발송내역 관리</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="10">
                  <template #title>
                    <el-icon><refresh-right/></el-icon>
                    <span>리턴 멤버쉽(RM) 서비스</span>
                  </template>
                  <el-menu-item index="10-1">RM 소개</el-menu-item>
                  <el-menu-item index="10-2">RM 메세지 발송</el-menu-item>
                  <el-menu-item index="10-3">RM 메세지 발송내역</el-menu-item>
                  <el-menu-item index="10-4">RM 통계</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="11">
                  <template #title>
                    <el-icon><right/></el-icon>
                    <span>다이렉트 RM(DRM)</span>
                  </template>
                  <el-menu-item index="11-1">DRM 소개</el-menu-item>
                  <el-menu-item index="11-2">DRM 설정</el-menu-item>
                  <el-menu-item index="11-3">DRM 통계</el-menu-item>
                </el-sub-menu>
                <el-sub-menu index="12">
                  <template #title>
                    <el-icon><phone/></el-icon>
                    <span>통합콜센터</span>
                  </template>
                  <el-menu-item index="12-1">통합콜센터 소개</el-menu-item>
                  <el-menu-item index="12-2">일반전화용(KT CRM형)</el-menu-item>
                  <el-menu-item index="12-3">인터넷전화용(LG CTI형)</el-menu-item>
                  <el-menu-item index="12-4">통합콜센터 서비스 신청</el-menu-item>
                </el-sub-menu>
                <el-menu-item index="13">
                  <el-icon><iphone /></el-icon>
                  <span>LGU+ 전국 대표번호</span>
                </el-menu-item>
                <el-sub-menu index="14">
                  <template #title>
                    <el-icon><circle-close/></el-icon>
                    <span>080 수신거부 서비스</span>
                  </template>
                  <el-menu-item index="14-1">080 수신거부 신청/확인</el-menu-item>
                  <el-menu-item index="14-2">080 수신거부 관리</el-menu-item>
                </el-sub-menu>
              </el-menu>
            </client-only>
          </el-scrollbar>
        </el-aside>
        <el-main class="main-container">
          <NuxtPage/>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script setup lang="ts">
import '/assets/css/global.css'
import {
  User, GoldMedal, Promotion, Coin, Message,
  Calendar, ShoppingCart, MessageBox,
  ChatDotRound, RefreshRight, Right, Phone, Iphone, CircleClose
} from "@element-plus/icons-vue";


</script>

<style scoped>
.header-line {
  box-sizing: border-box;
  border-bottom: 1px solid #e6e6e6;
  background: #fff;
}
.aside-area {
  position: fixed;
}
.aside-menu {
  border-right: 1px solid #e6e6e6;
  box-sizing: border-box;
  min-height: calc(100vh - 44px);
}
.main-container {
  margin-left: 250px;
}
</style>