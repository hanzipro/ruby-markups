# Plan — Rule 6 rb/rtc 措詞改寫：「非標準」→「引擎支援未到位」

Status: **awaiting owner go-ahead** — 定案前不動 SKILL.md。

> 2026-07-18 連動：skill 將統包、攤平到 repo 根——SKILL.md 移至根目錄
>（見 `issue1-unify-skill.md`）；「Rule 6」指現行編號，重組後本條屬
> **普世規則區**（rb/rtc 降階與注文語言無關）。SKILL.md 重組時一併
> 套用本 plan，不另跑一輪。

## 背景（owner 指示＋查證）

- xfq（W3C）私下告知：HTML 已發布新的 extension 標準，會持續推進 rb/rtc
  標準化。
- 查證：W3C《HTML Ruby Markup Extensions》
  https://www.w3.org/TR/html-ruby-extensions/
  已於 **2026-06-04 進入 Candidate Recommendation Snapshot**
  （editor: Florian Rivoal），明文 "this specification revokes this obsolete
  status, and deems these two elements fully conforming"。
- 因此 Rule 6 現行的「HTML Living Standard has marked `<rb>` and `<rtc>` as
  obsolete / non-conforming」在 W3C 層面已不成立。不用 rb/rtc 的真正理由是
  **跨引擎瀏覽器支援未到位**（tabular model／rtc 渲染不 interop），所以本
  spec 以「巢狀 ruby（ruby 包 ruby、再包 ruby）」作為多層注文、rbspan 式
  跨字注文的 interop 編碼。

## Edit list（SKILL.md）

1. **Rule 6 首段事實更正**：
   - WHATWG HTML 曾將 rb/rtc 列為 obsolete，但 W3C《HTML Ruby Markup
     Extensions》（CR Snapshot, 2026-06-04）已恢復其 fully conforming 地位，
     標準化持續推進中；附 TR 連結。
   - 本 spec 不用 rb/rtc 的理由改寫為：跨引擎支援未到位——tabular model 與
     rtc 多層渲染在各引擎間不 interop，故以巢狀 ruby 作 interop 編碼；
     引擎跟上後可望直接採 rb/rtc 形式。
2. **刪除**「rb/rtc 的歷史理由是人類可讀性、LLM 時代該理由已消失」段——
   rtc 的語義是多層注文，非僅可讀性；此說法不成立。
3. **converter 段改寫**（2026-07-18 定案）：
   - 函數改名：han.css 的 export 由 `convertLegacyRuby` 改為 **`transpileRuby`**
     （v4 未發布，無相容成本）。定名理由與 han 端執行細節見 han repo
     `docs/plans/transpile-ruby-rename.md`。
   - SKILL.md 此段引用一併更新為 `transpileRuby(src)`——順帶修掉現行文字
     寫 `convertLegacyZhuyinRuby`、實際 export 是 `convertLegacyRuby` 的錯名。
   - 「hand-written legacy … can be migrated」措詞改為中性的「既有
     rb/rtc／multi-rt 標記（existing markup）」——它們是 conforming，只是
     引擎未支援，故**降階（transpile）**成 interop 形。
   - forms 描述維持 A／B／C／E 並簡述四形（A tabular 注音、B tabular
     雙注文含 rbspan、C anonymous-base multi-rt、E 3+ 層）。
     註：D 在兩個 repo 均查無定義（字母跳號），不寫進 SKILL.md。
4. **Rule 6 標題斟酌**：由斷言式的 "`<rtc>` / `<rb>` are not used" 改為註明
   原因，如 "`<rtc>` / `<rb>` — not used (pending engine support)"。
5. **Authoring checklist item 7**：由「No `<rtc>` / `<rb>`」改為
   「No `<rtc>` / `<rb>`（引擎支援未到位，見 Rule 6——以巢狀 ruby 表達
   多層注文）」。

## 措詞原則

- 不再以「不符標準」為由勸退 rb/rtc；一律以「暫無跨引擎支援」為由。
- 明示巢狀 ruby 是**同一結構的 interop 編碼**，非對 rb/rtc 模型的否定。
