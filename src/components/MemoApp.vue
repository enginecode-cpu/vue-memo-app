<template>
  <div class="memo-app">
    <memo-form @addMemo="addMemo" />
    <ul class="memo-list">
      <memo
        v-for="memo in memos"
        :key="memo.id"
        :memo="memo"
        @deleteMemo="deleteMemo"
        @updateMemo="updateMemo"
      />
    </ul>
  </div>
</template>

<script>
import MemoForm from "./MemoForm";
import Memo from "./Memo";

export default {
  name: "MemoApp",
  data: () => {
    return {
      memos: []
    };
  },
  components: {
    MemoForm,
    Memo
  },
  created() {
    /**
     * 만약 기존에 추가된 localStorage에 데이터가 있다면
     * -> created 훅에서 localStorage의 데이터를 컴포넌트 내의
     *    memos 데이터에 넣어준다.
     * 그렇지 않으면
     * -> this.memos = []
     */
    this.memos = localStorage.memos ? JSON.parse(localStorage.memos) : [];
  },
  methods: {
    addMemo(payload) {
      this.memos.push(payload);
      this.storeMemo();
    },
    storeMemo() {
      const memoToString = JSON.stringify(this.memos);
      localStorage.setItem("memos", memoToString);
    },
    deleteMemo(id) {
      const targetIndex = this.memos.findIndex(v => v.id === id);
      this.memos.splice(targetIndex, 1);
      this.storeMemo();
    },
    updateMemo(payload) {
      const { id, content } = payload;
      const targetIndex = this.memos.findIndex(v => v.id === id);
      const targetMemo = this.memos[targetIndex];
      this.memos.splice(targetIndex, 1, { ...targetMemo, content });
      this.storeMemo();
    }
  }
};
</script>

<style scoped>
.memo-list {
  padding: 1.25rem;
  margin: 0;
}
</style>
