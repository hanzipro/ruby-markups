# Plan — issue #1 → 統包一份 skill＋攤平到 repo 根

Status: **done（2026-07-18，PR #6）**。

## 決策脈絡

- xfq（issue #1）指出 Scope「governs only zhuyin」與 Rule 8 涵蓋拼音／粵／
  日／韓自相矛盾，建議把其他語言移出。
- Owner 決定往**反方向**修：本 spec 只管「`<ruby>` HTML 怎麼寫」，layout
  差異（inter-character vs 頂上、fallback 幾何）是渲染實作、不歸本 spec 管；
  就 markup 而言大部分規則全類型共用，一份 skill 統包所有注文類型。
- README「pinyin／furigana／Jyutping／Latin-gloss 各自成 spec」的承諾**撤銷**
  ——推測性承諾（"may follow"）、未兌現、無外部依賴，撤掉零成本。
- 結構演進（2026-07-18 定案）：**repo 名 `ruby-markups` 不動**；統包後
  「一 repo 多 skill」的路已關掉，目錄層失去存在理由——SKILL.md 直接放
  **repo 根目錄**，安裝變成 clone 直裝（見下）。

## 新結構

1. **攤平到 repo 根**：`git mv zhuyin-ruby-markup/SKILL.md .`、
   `git mv zhuyin-ruby-markup/examples .`；`zhuyin-ruby-markup/` 目錄消失。
   - 安裝方式改為一條指令、`git pull` 即更新：
     ```sh
     git clone https://github.com/hanzipro/ruby-markups .claude/skills/ruby-markup
     ```
     資料夾名在 clone 時自取，天然與 frontmatter `name` 對齊；README 附
     `npx degit hanzipro/ruby-markups …` 作為不帶 `.git` 的替代。
   - 已知 trade-off（皆可接受）：裝入的資料夾帶 README／LICENSE／`docs/`
     ／`.git` 等非 skill 檔——skill 只載入 SKILL.md 與被引用檔，無害；
     未來若要第二份 skill 須再改結構——統包決策已排除此路。
   - ※ GitHub **不轉址檔案路徑**：xfq 在 issue #1 貼的
     `zhuyin-ruby-markup/SKILL.md` 連結會 404，回覆時附新路徑（根目錄
     `SKILL.md`）。
2. **SKILL.md 重組**：
   - frontmatter：`name: ruby-markup`；`description` 重寫——觸發詞擴及
     zhuyin／pinyin／furigana／jyutping／ruby 注文等，沿用 PR #3 的 block
     scalar 寫法，守住 ≤1024 字元、無 XML tags。
   - **普世規則區**（取自現行 Rule 2／3／6／7／8）：標點禁則、巢狀多層
     注文、rb/rtc 降階（transpile，套 rule6 plan）、children 精簡、
     讀音標準確認＋多音字處理（套 issue2 plan 的 step 0）。
   - **各類型專節**：
     - 注音——最厚的一節（現行 Rule 1／4／5：一字一 ruby 的 fallback
       幾何、`zhuyin|mps` class、調號順序＋教育部辭典形預設）。
     - 拼音／furigana／粵拼——先立骨架節（各自的切分粒度、class 慣例、
       注文內容慣例），內容漸進補。
   - 未來某類型寫深時用 progressive disclosure（per-type reference 檔），
     SKILL.md 本體守 500 行內。
   - Rule 編號必然重排——其他 plan 對「Rule N」的引用以**內容**為準。
3. **README 重寫**：撤 per-spec 表格；改述「一份 spec＝普世規則＋各類型
   專節」；安裝段改 clone 直裝（含 degit 替代）；reference implementation
   段保留。

## 執行順序（與其他 plan 的關係）

1. `git mv` 攤平＋README 重寫。
2. SKILL.md 重組時**一併**套入 `rule6-rbrtc-rewording.md` 與
   `issue2-reading-standard.md` 的內容——一次改到位，避免多輪 diff。
3. 上游定稿後，同步 han 的 vendored 副本並改資料夾名
   `.claude/skills/zhuyin-ruby-markup/` → `.claude/skills/ruby-markup/`
   （見 han repo `docs/plans/transpile-ruby-rename.md`）。

## 回覆稿（owner 確認後貼到 issue #1）

> 想了一輪，決定往你建議的**反方向**修，但同樣消掉這個矛盾：這份 spec 會
> 正式擴成 CJK ruby markup 通則，不再自稱 zhuyin-only。
>
> 理由：README 原本說各注文類型「各自成 spec」，但那其實是把 layout 實作
> 差異（inter-character vs 頂上、CSS fallback 幾何）當成了分 spec 的理由——
> 那些是渲染引擎／CSS 的事，本 spec 只管「`<ruby>` HTML 怎麼寫」。就
> markup 而言，大部分規則本來就是全類型共用的：標點禁則（Rule 2）、巢狀
> 多層注文（Rule 3）、rb/rtc 的 interop 處理（Rule 6）、多音字須經作者
> 確認才能入稿（Rule 8）。各類型真正不同的只有切分粒度、class 命名、注文
> 內容慣例——各一節就寫完。
>
> 所以改法是：repo 攤平成單一 spec（SKILL.md 移到根目錄，clone 即可直接
> 裝進 `.claude/skills/`），重組成「普世規則＋各類型專節」；注音是著墨
> 最深的專節（它的排版約束最多：inter-character、一字一 ruby、調號），
> 拼音、furigana、粵拼各立專節骨架。Scope 一段隨之重寫，「governs only
> zhuyin」的矛盾就不存在了。
>
> （你開頭貼的 `zhuyin-ruby-markup/SKILL.md` 連結屆時會失效，新路徑是
> 根目錄的 `SKILL.md`。）

## 不做

- **不改 repo 名**（`ruby-markups` 維持）。
- 不把拼音／粵／日／韓內容搬出 skill（與 xfq 原建議相反，理由如上）。
- 不在本輪把拼音／furigana／粵拼專節寫深——先立骨架，隨需求補。
