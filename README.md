# zoohoson.github.io

HOLOG 기술 블로그. Jekyll 4 + [Hydejack](https://github.com/hydecorp/hydejack) v9.2.1 Free(gem 테마, GPL-3.0), GitHub Actions로 빌드·배포.

- 2026-11: Hydejack 6.4.0(2017, 테마 파일 직접 포함) → v9.2.1 gem. 메뉴 체계·사이드바 모양·URL은 v6 그대로 유지
- 배포: `master` 푸시 → `.github/workflows/pages-deploy.yml`(빌드 → html-proofer → Pages 배포). Pages Source는 "GitHub Actions"

## 1. 로컬 실행

CI와 같은 Ruby 3.4를 쓴다. 시스템 Ruby 2.6, Homebrew 기본 `ruby`(4.x)는 쓰지 않는다.

```bash
cd ~/pjt/techlog/zoohoson.github.io
export PATH="/opt/homebrew/opt/ruby@3.4/bin:$PATH"   # 최초: brew install ruby@3.4
bundle config set --local path vendor/bundle         # 최초 1회
bundle install
bundle exec jekyll serve                             # http://127.0.0.1:4000 (초안 포함: --drafts)
```

- `_config.yml`을 바꾸면 서버를 재시작해야 반영된다
- 배포 전 점검(CI와 동일)

  ```bash
  JEKYLL_ENV=production bundle exec jekyll build
  bundle exec htmlproofer _site --disable-external --allow-missing-href
  ```

## 2. 글 작성 규칙

- 위치·파일명: `_posts/` 아래 `YYYY-MM-DD-<slug>.md`. 폴더는 URL과 무관하며 관례는 `dlog/<태그>/`, `book/`, `article/<연도>/`. 초안은 `_drafts/`
- URL: `/<카테고리>/YYYY/MM/DD/<slug>/`. slug는 영문 소문자·하이픈·핵심 키워드(예: `mlp-int8-quantization`). 게시 후 바꾸지 않는다
- 카테고리·태그(사이드바 메뉴와 1:1)

  | 메뉴 | `categories` | `tags` |
  |---|---|---|
  | Dlog > Algorithm · ML · Git · Linux · Web · Proteomics | `dlog` | `algorithm` · `ml` · `git` · `linux` · `web` · `proteomics` |
  | Documents > Article(논문 리뷰) · Book(서평) | `doc` | `article` · `book` |

- front matter 템플릿

  ```yaml
  ---
  layout: post
  title: 'MLP 직접 구현과 INT8 양자화: 정확도·속도 트레이드오프'   # 검색어 + 구체성
  description: >                                                  # 80~120자, 검색 결과 스니펫
    PyTorch로 MLP를 직접 구현하고 동적·정적 INT8 양자화를 적용해 정확도, 모델 크기, CPU 지연시간을 비교한다.
  categories: dlog
  tags: ml
  image: /assets/dlog/2026/mlp-quantization-pareto.png              # 공유 미리보기(og:image)
  comments: true
  # 수식: 현재 렌더링 꺼짐(_config.yml kramdown 주석 참고). 첫 수식 글 전에 KaTeX/MathJax 결정
  # redirect_from: [/old/url/]                                      # URL을 바꿔야 할 때만
  ---
  ```

- 이미지: `assets/<카테고리>/<연도>/`에 의미 있는 파일명으로 두고 alt를 쓴다
- 비공개: `published: false`. `_posts/article/`의 옛 뉴스 스크랩 113건은 전부 비공개, 이미지(`assets/img/2xxxxx*`)도 배포 제외

## 3. 운영 메모

- 사이드바 메뉴: `menu: true` 페이지·카테고리를 `order` 순으로, `submenu: true` 카테고리 아래에 `_featured_tags`(front matter `category: <slug>`)를 접이식(▾)으로 표시. 태그는 글 수와 관계없이 모두 표시
- 검색: tipuesearch(`/search/`). 인덱스는 빌드 시 공개 글로 생성되고, 스크립트는 검색 페이지에서만 로드
- 댓글: Disqus(`zooho`). identifier는 v6와 같은 `page.id`
- 분석: GA4 `G-4QJJTRVPXK`(push-state 페이지 전환은 GA4 향상된 측정이 수집)
- 테마 덮어쓰기(Hydejack 업그레이드 시 gem 원본과 비교)

  | 파일 | 이유 |
  |---|---|
  | `_includes/body/nav.html` | 접이식 하위 메뉴(v6 커스텀 이식) |
  | `_includes/body/sidebar-sticky.html` | v6 사이드바 배치: 메뉴 → 검색창 → 프로필 → 소개 → 소셜 |
  | `_includes/body/analytics.html` | 원본은 UA 전용 → GA4 |
  | `_includes/my-comments.html` | Disqus identifier를 v6 값(`page.id`)으로 유지 |
  | `_includes/my-head.html` | 네이버 서치어드바이저 meta |
  | `_layouts/list.html` | 글 0건 태그에서 전체 글이 나오지 않게 |
  | `_sass/my-inline.scss` | v6 사이드바 모양: 왼쪽 정렬, 메뉴 구분선, 하위 메뉴, 흰 검색창, 제목 장식선·링크 밑줄 제거 |

## 4. 검색 노출(SEO) 가이드

### 4.1 요약

- 순위를 올리는 것은 콘텐츠다. 기술 작업은 감점을 막는 역할이고, 사이트 기술 기반은 2026-11 테마 이전으로 갖춰졌다
- 전략: 실제로 검색되는 롱테일 질문 하나에 글 한 편으로 답하고, 직접 실험한 근거(코드·측정값·환경·실패 사례)를 담는다
- Google AI Overviews·AI Mode도 같은 색인·랭킹을 쓴다. 별도 GEO 작업이나 `llms.txt`는 필요 없다(Google 가이드 2026-05)

### 4.2 사이트 기반 현황 (2026-11-25)

| 항목 | 상태 |
|---|---|
| 메타·구조화 데이터 | jekyll-seo-tag: title·description·canonical·OG, JSON-LD(홈 `WebSite`, 글 `BlogPosting`) |
| 사이트맵·robots | jekyll-sitemap 자동 생성(`/sitemap.xml`, git 기반 `lastmod`), robots `Allow: /` + Sitemap |
| 언어·분석 | `<html lang="ko">`, GA4 1회 삽입 |
| 품질 신호 | 뉴스 전재 113건 비공개, 중복 URL 제거. 사이트 단위 신호도 쓰이므로 저품질 글은 다시 공개하지 않는다 |
| 성능 | 666KB 검색 인덱스·jQuery를 매 페이지에서 로드하지 않음 |
| 소유 확인 | Google(HTML 파일), 네이버(meta + HTML 파일) |
| **남은 일** | Search Console·네이버에 사이트맵 재제출, 미색인 사유 확인 / Bing Webmaster Tools 등록(Search Console 가져오기) |

### 4.3 쓰기 전: 주제·키워드

- 수요 확인: Google 자동완성·관련 검색어, Search Console 실적(이미 노출되는 쿼리), Google Trends, 네이버 데이터랩·검색광고 키워드 도구
- 롱테일 우선: "트랜스포머"(경쟁 과다) 대신 "PyTorch 트랜스포머 직접 구현 양자화 속도 비교"
- 한·영 병기: 기술 용어는 영문 검색이 많다 → `양자화(Quantization)`처럼 제목·본문에 함께 쓴다
- 토픽 클러스터: 허브 글(총정리)과 시리즈 각 편, 관련 논문 리뷰(Article)를 서로 링크한다

### 4.4 쓸 때: 페이지 체크리스트

- [ ] title: 핵심 키워드를 앞쪽에 두고 구체적으로. 예: `[MLops] - overview` → `MLOps 파이프라인 개요: 데이터·모델·코드 관리 포인트`
- [ ] description: 80~120자로 직접 쓴다(없으면 첫 단락이 잘려 들어간다)
- [ ] slug: 영문 소문자·하이픈·키워드. 게시 후 바꾸지 않고, 꼭 바꿔야 하면 `redirect_from`을 쓴다
- [ ] 첫 문단에 결론 3줄. 스니펫과 AI 요약은 여기서 인용되기 쉽다
- [ ] H2 → H3 계층, 표·코드블록 사용(Hydejack Free에는 자동 목차가 없으므로 긴 글은 본문 앞에 목차 링크를 직접 쓴다)
- [ ] 경험 근거(E-E-A-T의 Experience): 실행 환경·버전, 측정 방법, 재현 저장소 링크, 실패 사례
- [ ] 이미지: 의미 있는 파일명, alt, 적정 해상도(가능하면 WebP), 대표 이미지는 `image:`로 지정
- [ ] 내부 링크: 시리즈 이전·다음 편, 허브 글, 관련 논문 리뷰
- [ ] 버전에 의존하는 글은 고칠 때 본문에 수정 내용을 적는다(수정일은 git 기반으로 자동 표시)

### 4.5 게시 직후

- Search Console > URL 검사 > 색인 생성 요청
- 네이버 서치어드바이저 > 요청 > 웹 페이지 수집
- 공유: LinkedIn·GeekNews·Velog 등에는 요약과 원문 링크만 올린다. 전문을 복제하면 중복 콘텐츠가 된다. canonical을 지정할 수 있는 곳(Medium import)만 전문 게시
- 실험 코드 저장소 README와 글을 서로 링크한다

### 4.6 측정·개선 (월 1회)

- Search Console 실적: 쿼리별 노출·클릭·CTR·평균 순위
  - 노출이 많은데 CTR이 낮다 → title·description 개선
  - 평균 순위 8~20위인 쿼리 → 해당 글 보강, 내부 링크 추가
  - '페이지 색인 생성' 보고서에서 제외 사유 확인
- GA4: 유입 경로, 체류 시간
- 기대치(일반적 관측): 신규 글 색인 수일~수주, 순위 안정 수개월

### 4.7 하지 말 것

- 내 검색·반복 클릭으로 순위 올리기: 결과가 개인화돼 착시일 뿐이고, 인위적 클릭은 조작 행위다. 순위는 Search Console로 본다
- `<meta name="keywords">`: Google은 쓰지 않는다
- robots.txt로 색인 제외: 크롤링만 막는다. 색인에서 빼려면 `noindex` 메타 또는 `published: false`
- 링크 구매·상호 링크 교환, 검토 없는 AI 대량 생성: 스팸 정책 위반(scaled content abuse는 2026년 스팸 업데이트의 단속 대상). AI 활용 자체는 허용된다
- 게시된 글의 URL·카테고리 변경: URL에 카테고리가 들어가므로 카테고리를 바꾸면 URL도 바뀐다

### 4.8 출처

- [Google SEO 기본 가이드](https://developers.google.com/search/docs/fundamentals/seo-starter-guide) · [유용한 사용자 중심 콘텐츠](https://developers.google.com/search/docs/fundamentals/creating-helpful-content)
- [생성형 AI 검색 기능 최적화 가이드(2026-05)](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)
- [스팸 정책](https://developers.google.com/search/docs/essentials/spam-policies) · [Site Reputation 정책 업데이트(2026-08)](https://developers.google.com/search/blog/2026/08/update-site-reputation-policy)
- [robots.txt 소개](https://developers.google.com/search/docs/crawling-indexing/robots/intro) · [사이트맵(priority·changefreq는 무시)](https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap) · [keywords 메타 미사용](https://developers.google.com/search/blog/2009/09/google-does-not-use-keywords-meta-tag)
- [네이버 서치어드바이저](https://searchadvisor.naver.com/) · [Bing Webmaster Tools](https://www.bing.com/webmasters)
