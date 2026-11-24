source "https://rubygems.org"

# 로컬·CI 모두 Ruby 3.4 + Jekyll 4.x (GitHub Actions 빌드). 실행법은 README.md
gem "jekyll", "~> 4.3"
gem "jekyll-theme-hydejack", "~> 9.2"

# Ruby 3에서 `jekyll serve`에 필요
gem "webrick"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-paginate"
  gem "jekyll-redirect-from"
  gem "jekyll-seo-tag"
  gem "jekyll-sitemap"
  gem "jekyll-include-cache"
  gem "jekyll-last-modified-at"
end

gem "html-proofer", "~> 5.0", group: :test
