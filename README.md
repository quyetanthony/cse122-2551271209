Dưới đây là bài phân tích trang chủ **`https://tlu.edu.vn/` (Trường Đại học Thủy lợi)** theo **tư duy 3 lớp song song (Layout Box – CSS Layout – Content & Semantic HTML)**, dựa trên dữ liệu quan sát trực tiếp từ giao diện desktop (`1440×900`), quan sát responsive (`862×789`) và cấu trúc DOM thực tế của trang.

> **Phân biệt nguồn thông tin trong bài:**
> - **Quan sát trực tiếp:** Các vùng, tỷ lệ cột, số lượng phần tử, trạng thái sticky/fixed, thanh menu trên desktop (`1440×900`) và breakpoint ẩn menu (`< 1025px` tại `862×789`).
> - **Suy luận kỹ thuật / Thiết kế sư phạm:** Hành vi chi tiết ở màn hình mobile nhỏ (`~375px`) cho các section giữa và cách tái cấu trúc bằng **Flexbox thuần** song song với **CSS Grid** để người học nắm chắc bản chất dàn trang.

---

# BƯỚC 1 — BẢN ĐỒ BỐ CỤC TỔNG THỂ TOÀN TRANG

Nhìn từ ngoài vào trong và từ trên xuống dưới, trang chủ `tlu.edu.vn` sử dụng mô hình **Full-bleed Band + Centered Container** (vùng nền trải tràn 100% màn hình, nội dung bên trong khóa khung căn giữa tối đa `1400px` với `margin: 0 auto`):

```text
page-wrapper
├── site-header (sticky top)
│   ├── header-top (nền trắng, container 1400px)
│   └── main-nav (nền xanh #0118d8, container 1400px)
├── floating-social (fixed mép phải màn hình)
├── main-content (flex: 1)
│   ├── section-0: hero-slider (full-bleed)
│   ├── section-1: news-notices (container 1400px)
│   ├── section-2: events-carousel (container 1400px)
│   ├── section-3: statistics (container 1440px)
│   ├── section-4: highlight-banner (full-bleed)
│   ├── section-5: admission-intro (container 1400px)
│   ├── section-6: triad-showcase [Đào tạo / Sinh viên / Cựu SV] (container 1400px)
│   ├── section-7: academic-units (full-bleed xanh, nội dung căn giữa)
│   ├── section-8: international-coop (container 1400px)
│   ├── section-9: research (container 1400px)
│   ├── section-10: partner-network (full-bleed xanh, container 1400px)
│   ├── section-11: community (container 1400px)
│   └── section-12: tlu-moments (container 1400px)
└── site-footer (full-bleed xanh đậm #1907dc, container 1400px)
    ├── footer-top
    ├── footer-middle
    └── footer-bottom
```

| Vùng lớn | Tên layout đề xuất | Vai trò | Phạm vi | Quan hệ với cha | Cần `.container`? |
|---|---|---|---|---|---|
| Đầu trang | `site-header` | Nhận diện thương hiệu, tìm kiếm, đổi ngôn ngữ, điều hướng | Dính cố định trên cùng (`sticky`, cao `128px`) | Con trực tiếp của `page-wrapper` | **Có** (`1400px` cho cả 2 hàng) |
| MXH nổi | `floating-social` | Truy cập nhanh 4 kênh MXH | Trôi dọc ở mép phải (`fixed`) | Con trực tiếp của `page-wrapper` | **Không** |
| Slider đầu trang | `hero-slider` | Quảng bá thông điệp/sự kiện lớn | Tràn ngang toàn màn hình (cao `~445px`) | Con đầu tiên của `main-content` | **Không** (full-bleed) |
| Tin tức & Thông báo | `news-notices` | Tin nổi bật + thông báo mới | 2 khối bất đối xứng (`2:1`) | Con của `main-content` | **Có** (`1400px`) |
| Sự kiện | `events-carousel` | Trình chiếu các sự kiện sắp/đang diễn ra | 4 thẻ ngang trong track trượt | Con của `main-content` | **Có** (`1400px`) |
| Con số ấn tượng | `statistics` | Chứng minh quy mô & thành tựu | 5 cột ngang đều nhau | Con của `main-content` | **Có** (`1440px`) |
| Banner nhấn | `highlight-banner` | Banner hình ảnh toàn chiều rộng | Tràn viền 100%, nền `#0118d8` | Con của `main-content` | **Không** (full-bleed) |
| Tuyển sinh | `admission-intro` | Giới thiệu tuyển sinh & nút đăng ký | 2 cột 50/50 (ảnh trái, chữ phải) | Con của `main-content` | **Có** (`1400px`) |
| Khối 3 chuyên mục | `triad-showcase` | Đào tạo – Sinh viên – Cựu sinh viên | 3 cột bằng nhau (`1:1:1`) | Con của `main-content` | **Có** (`1400px`) |
| Đơn vị ĐT, KHCN | `academic-units` | Chọn khoa/phân hiệu tương tác | Tràn nền xanh, nội dung 2 cột | Con của `main-content` | **Có** (cho khối nội dung) |
| Khối tin 3+3 (×3) | `showcase-row` | Hợp tác QT, Nghiên cứu, Vì cộng đồng | 3 thẻ ảnh + 1 cột 3 tin chữ | Con của `main-content` | **Có** (`1400px`) |
| Mạng lưới đối tác | `partner-network` | Giới thiệu & logo 23 đối tác | Tràn nền `#0118d8` + dải trắng | Con của `main-content` | **Có** (`1400px`) |
| Khoảnh khắc TLU | `tlu-moments` | Bộ sưu tập ảnh hoạt động | Lưới Bento 5 cột × 4 hàng (15 ảnh) | Con của `main-content` | **Có** (`1400px`) |
| Chân trang | `site-footer` | Liên hệ, điều hướng phụ, video, bản quyền | Tràn nền `#1907dc`, 3 tầng dọc | Con cuối của `page-wrapper` | **Có** (`1400px`) |

---

# BƯỚC 2 — PHÂN TÍCH TỪNG COMPONENT THEO 3 LỚP SONG SONG

---

## 01. Header Top Bar (`header-top`)

### 1. Nhận diện Layout Box
- **Tên hộp:** `header-top`
```text
site-header
└── header-top
    └── container (header-top__inner)
        ├── brand-logo
        └── header-controls
            ├── search-form
            │   ├── search-input
            │   └── search-submit
            └── lang-switcher
                ├── lang-label
                └── lang-flag
```
- **Giải thích:** Tách `header-top` khỏi thanh menu xanh bên dưới để quản lý độc lập vùng nhận diện thương hiệu và tiện ích. `header-controls` gom khung tìm kiếm + nút chuyển ngôn ngữ thành một nhóm bên phải để dễ đẩy sát mép phải bằng Flexbox.

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Flexbox** (2 cấp lồng nhau).
- **Flex Container:** `.header-top__inner` → **Flex Items:** `.brand-logo`, `.header-controls`.
- **Flex Container con:** `.header-controls` → **Flex Items:** `.search-form`, `.lang-switcher`.
- **Không cần Grid** vì các phần tử chỉ nằm trên **một hàng ngang duy nhất** (trục 1 chiều).

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Logo + chữ ĐẠI HỌC THỦY LỢI | Thương hiệu & link về trang chủ | `a`, `img` |
| Ô nhập `"Tìm kiếm..."` | Trường nhập liệu tìm kiếm | `form`, `input[type="search"]` |
| Icon kính lúp | Nút gửi biểu mẫu | `button[type="submit"]`, `svg` |
| Nút `VN` + cờ đỏ sao vàng | Công tắc chuyển ngôn ngữ | `button[type="button"]`, `span`, `img` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="header-top">
    <div class="container header-top__inner">
        <div class="brand-logo"></div>
        <div class="header-controls">
            <div class="search-form">
                <div class="search-input"></div>
                <div class="search-submit"></div>
            </div>
            <div class="lang-switcher">
                <div class="lang-label"></div>
                <div class="lang-flag"></div>
            </div>
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<div class="header-top">
    <div class="container header-top__inner">
        <a href="/" class="brand-logo">
            <img src="logo-tlu.png" alt="Trường Đại học Thủy lợi">
        </a>
        <div class="header-controls">
            <form class="search-form" role="search">
                <input type="search" class="search-input" placeholder="Tìm kiếm..." aria-label="Tìm kiếm">
                <button type="submit" class="search-submit" aria-label="Tìm kiếm">🔍</button>
            </form>
            <button type="button" class="lang-switcher" aria-label="Chuyển sang tiếng Anh">
                <span>VN</span>
                <img src="flag-vn.svg" alt="">
            </button>
        </div>
    </div>
</div>
```

### 6. CSS Layout minh họa
```css
.header-top__inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 16px;
    height: 100px;
}

.header-controls {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-left: auto;
}

.search-form {
    display: flex;
    align-items: center;
}

.search-input {
    flex: 1;
}
```

### 7. Kết luận tư duy
```text
header-top
Layout : brand-logo + header-controls (search-form + lang-switcher)
CSS    : Flexbox row (space-between, align-items: center)
HTML   : div.header-top > a(img) + div(form[input, button] + button)
```

---

## 02. Main Navigation (`main-nav`)

### 1. Nhận diện Layout Box
- **Tên hộp:** `main-nav`
```text
site-header
└── main-nav
    └── container (main-nav__inner)
        └── nav-list
            ├── nav-item (GIỚI THIỆU)
            ├── nav-item (TIN TỨC)
            ├── nav-item (TUYỂN SINH)
            ├── nav-item (ĐÀO TẠO)
            ├── nav-item (NGHIÊN CỨU)
            ├── nav-item (HỢP TÁC QUỐC TẾ)
            ├── nav-item (SINH VIÊN)
            └── nav-item (eTLU)
```
- **Giải thích:** Thanh menu trải nền xanh dương `#0118d8` toàn màn hình, nhưng 8 mục menu nằm gọn trong `.container` để thẳng hàng với logo và nội dung bên dưới.

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Flexbox** (`nav-list` là Flex Container; 8 `nav-item` là Flex Items; bên trong mỗi `nav-item` có thể lồng Flexbox để căn chữ + mũi tên dropdown).
- **Thuộc tính then chốt:** `align-items: stretch` giúp mọi `nav-item` cao bằng 100% thanh nav (`40px`) để vạch ngăn dọc giữa các mục liền mạch từ trên xuống dưới. `flex: 1 1 auto` chia đều khoảng trống theo độ rộng nội dung chữ.
- *(Ghi chú quan sát)*: Ở viewport `< 1025px` (ví dụ `862px`), thanh này ẩn đi và nhường chỗ cho nút hamburger `"Mở menu"` trên `header-top`.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Thanh điều hướng chính | Vùng điều hướng cấp site | `nav[aria-label="Điều hướng chính"]` |
| Tập hợp 8 danh mục | Danh sách liên kết | `ul`, `li` |
| 7 mục chuyển trang (Giới thiệu... Sinh viên) | Điều hướng đến trang con | `a` |
| Mục `eTLU` (không chuyển URL trực tiếp, mở menu) | Nút kích hoạt menu xổ xuống | `button[aria-haspopup="menu"]` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="main-nav">
    <div class="container">
        <div class="nav-list">
            <div class="nav-item">GIỚI THIỆU</div>
            <div class="nav-item">TIN TỨC</div>
            <div class="nav-item">TUYỂN SINH</div>
            <div class="nav-item">ĐÀO TẠO</div>
            <div class="nav-item">NGHIÊN CỨU</div>
            <div class="nav-item">HỢP TÁC QUỐC TẾ</div>
            <div class="nav-item">SINH VIÊN</div>
            <div class="nav-item">eTLU</div>
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<nav class="main-nav" aria-label="Điều hướng chính">
    <div class="container">
        <ul class="nav-list">
            <li class="nav-item"><a href="/gioi-thieu">GIỚI THIỆU</a></li>
            <li class="nav-item"><a href="/tin-tuc-thong-bao">TIN TỨC</a></li>
            <li class="nav-item"><a href="https://ts.tlu.edu.vn/">TUYỂN SINH</a></li>
            <li class="nav-item"><a href="/dao-tao">ĐÀO TẠO</a></li>
            <li class="nav-item"><a href="/nghien-cuu">NGHIÊN CỨU</a></li>
            <li class="nav-item"><a href="/doi-ngoai">HỢP TÁC QUỐC TẾ</a></li>
            <li class="nav-item"><a href="/sinh-vien">SINH VIÊN</a></li>
            <li class="nav-item"><button type="button" aria-haspopup="menu">eTLU</button></li>
        </ul>
    </div>
</nav>
```

### 6. CSS Layout minh họa
```css
.nav-list {
    display: flex;
    align-items: stretch;
    width: 100%;
    height: 40px;
}

.nav-item {
    flex: 1 1 auto;
    display: flex;
    align-items: center;
    justify-content: center;
}
```

### 7. Kết luận tư duy
```text
main-nav
Layout : nav-list -> 8 nav-item dàn ngang đều
CSS    : Flexbox row (align-items: stretch, flex: auto)
HTML   : nav > ul > li > a (hoặc button cho dropdown)
```

---

## 03. Floating Social Bar (`floating-social`)

### 1. Nhận diện Layout Box
```text
page-wrapper
└── floating-social
    ├── social-btn (Facebook)
    ├── social-btn (YouTube)
    ├── social-btn (Instagram)
    └── social-btn (TikTok)
```
- **Giải thích:** Khối nổi cố định ở cạnh phải màn hình (`top: 24vh; right: 16px`), tách khỏi luồng tài liệu để luôn hiện hữu khi người dùng cuộn trang dài `~7950px`.

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Position (`fixed`) + Flexbox (`column`)**.
- **Flex Container:** `.floating-social` → **Flex Items:** 4 `.social-btn`.
- Bản thân mỗi `.social-btn` cũng là Flex Container nhỏ để căn giữa icon theo cả 2 trục.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Thanh liên kết MXH | Cụm điều hướng phụ trợ | `aside` hoặc `nav[aria-label="Mạng xã hội"]` |
| Danh sách 4 kênh | Nhóm nút liên kết | `ul`, `li` |
| Liên kết từng MXH | Mở trang ngoài (FB, YT, IG, TikTok) | `a[target="_blank"]`, `svg` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="floating-social">
    <div class="social-btn"></div>
    <div class="social-btn"></div>
    <div class="social-btn"></div>
    <div class="social-btn"></div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<aside class="floating-social" aria-label="Kênh mạng xã hội">
    <ul class="floating-social__list">
        <li><a href="https://facebook.com/..." class="social-btn" aria-label="Facebook">...</a></li>
        <li><a href="https://youtube.com/..." class="social-btn" aria-label="YouTube">...</a></li>
        <li><a href="https://instagram.com/..." class="social-btn" aria-label="Instagram">...</a></li>
        <li><a href="https://tiktok.com/..." class="social-btn" aria-label="TikTok">...</a></li>
    </ul>
</aside>
```

### 6. CSS Layout minh họa
```css
.floating-social {
    position: fixed;
    right: 16px;
    top: 24vh;
    z-index: 20;
}

.floating-social__list {
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.social-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
}
```

### 7. Kết luận tư duy
```text
floating-social
Layout : 4 icon xếp dọc bên phải
CSS    : position: fixed + Flexbox column (gap: 8px)
HTML   : aside > ul > li > a(svg)
```

---

## 04. Hero Banner Slider (`hero-slider`)

### 1. Nhận diện Layout Box
```text
main-content
└── hero-slider
    ├── hero-track
    │   └── hero-slide (x7)
    └── hero-pagination
        └── hero-dot (x7)
```
- **Giải thích:** Khối mở đầu trang dạng băng chuyền ảnh toàn màn hình (`~445px` cao). Theo quan sát trực tiếp, không có text/nút phụ đè lên giữa banner; chỉ có dãy nút chấm phân trang nằm ở cạnh dưới.

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Flexbox (track ngang) + Position (`relative` / `absolute`)**.
- `.hero-slider` làm mốc (`position: relative; overflow: hidden;`).
- `.hero-track` là **Flex Container** (`display: flex; flex-wrap: nowrap;`) giữ 7 `.hero-slide` (`flex: 0 0 100%`) nằm nối đuôi nhau theo chiều ngang để trượt bằng `transform: translateX(...)`.
- `.hero-pagination` dùng `position: absolute; bottom: 16px; left: 50%; transform: translateX(-50%);` kết hợp `display: flex; gap: 8px;` để căn giữa các chấm.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Vùng banner trang chủ | Khu vực trình chiếu nổi bật | `section[aria-label="Nội dung nổi bật"]` |
| Ảnh từng banner (hỗ trợ nhiều kích thước) | Hình ảnh truyền thông | `picture`, `source`, `img` |
| 7 chấm chuyển slide | Nút điều khiển giao diện | `button[aria-label="Đi đến banner 1..7"]` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="hero-slider">
    <div class="hero-track">
        <div class="hero-slide"></div>
        <!-- lặp 7 slide -->
    </div>
    <div class="hero-pagination">
        <div class="hero-dot"></div>
        <!-- lặp 7 dot -->
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="hero-slider" aria-label="Nội dung nổi bật trang chủ">
    <div class="hero-track">
        <figure class="hero-slide">
            <picture>
                <img src="banner-1.webp" alt="Banner 1">
            </picture>
        </figure>
    </div>
    <div class="hero-pagination" role="tablist" aria-label="Chọn banner">
        <button type="button" class="hero-dot" aria-label="Đi đến banner 1"></button>
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.hero-slider {
    position: relative;
    width: 100%;
    overflow: hidden;
}

.hero-track {
    display: flex;
}

.hero-slide {
    flex: 0 0 100%;
}

.hero-pagination {
    position: absolute;
    bottom: 16px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 8px;
}
```

### 7. Kết luận tư duy
```text
hero-slider
Layout : hero-track (7 slides) + hero-pagination (overlay đáy)
CSS    : Position relative/absolute + Flexbox row (flex: 0 0 100%)
HTML   : section > figure(picture > img) + button(dots)
```

---

## 05. Tin tức & Thông báo (`news-notices-section`)

### 1. Nhận diện Layout Box
```text
main-content
└── news-notices-section
    └── container (news-notices-layout)
        ├── news-column
        │   ├── section-heading (TIN TỨC)
        │   └── news-split
        │       ├── news-featured (news-card--large)
        │       └── news-side
        │           ├── news-card--compact (x2)
        │           └── news-pager
        └── notices-column
            ├── section-heading (THÔNG BÁO)
            └── notices-list
                └── notice-card (x6)
```
- **Giải thích:** Đây là cụm thông tin trọng tâm đầu trang với bố cục **bất đối xứng 2 tầng**: ngoài chia `2 : 1` giữa Tin tức và Thông báo; trong cột Tin tức lại chia `2 : 1` giữa 1 bài nổi bật có thẻ `HOT` và 2 bài phụ có ảnh nhỏ hơn.

### 2. Tư duy dàn trang CSS
- **Layout mode chính:**
  - Trang thực tế dùng **CSS Grid** (`grid-cols-3` với `col-span-2` ở ngoài, và `grid-cols-[2fr_1fr]` bên trong).
  - Nếu phân tích theo **tư duy Flexbox**:
    - `.news-notices-layout`: Flex Container (`display: flex; gap: 40px; align-items: flex-start;`), `.news-column` đặt `flex: 2;` và `.notices-column` đặt `flex: 1;` → **Tỷ lệ 2 : 1**.
    - `.news-split`: Flex Container (`display: flex; gap: 30px;`), `.news-featured` đặt `flex: 2;` và `.news-side` đặt `flex: 1;` → **Tỷ lệ 2 : 1**.
    - `.notices-list`: **Không cần Flexbox/Grid phức tạp** — dùng **Normal Flow** (hoặc `flex-direction: column; gap: 12px;` chỉ để tạo khoảng cách đều giữa 6 thẻ).

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Tiêu đề `TIN TỨC` / `THÔNG BÁO` | Tiêu đề mục + link sang trang danh sách | `h2 > a` |
| Mỗi thẻ tin tức (ảnh, tag, tiêu đề, trích dẫn, ngày) | Bài viết độc lập | `article` |
| Ảnh minh họa tin | Hình ảnh bài viết | `img` |
| Nhãn `TIN TỨC CHUNG`, `HOT` | Phân loại / trạng thái | `span` |
| Tiêu đề bài viết + link | Đọc chi tiết | `h3 > a` |
| Đoạn mô tả ngắn | Tóm tắt bài | `p` |
| Ngày đăng (`19/10/2026`) | Thời gian xuất bản | `time` |
| Danh sách 6 thông báo | Danh sách tin văn bản | `ul > li > a` + `time` |
| Nút `Tin trước`, `Tin tiếp theo`, 3 chấm | Điều hướng trang tin | `button` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="news-notices-section">
    <div class="container news-notices-layout">
        <div class="news-column">
            <div class="section-heading">TIN TỨC</div>
            <div class="news-split">
                <div class="news-featured">
                    <div class="news-card">...</div>
                </div>
                <div class="news-side">
                    <div class="news-card">...</div>
                    <div class="news-card">...</div>
                </div>
            </div>
        </div>
        <div class="notices-column">
            <div class="section-heading">THÔNG BÁO</div>
            <div class="notices-list">
                <div class="notice-card">...</div> <!-- x6 -->
            </div>
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="news-notices-section">
    <div class="container news-notices-layout">
        <div class="news-column">
            <h2 class="section-heading"><a href="/tin-tuc-thong-bao">TIN TỨC</a></h2>
            <div class="news-split">
                <article class="news-featured">
                    <img src="thumb-hot.jpg" alt="...">
                    <span class="badge">TIN TỨC CHUNG</span>
                    <h3><a href="...">Thông báo Tuyển dụng giảng viên...</a></h3>
                    <p>Căn cứ nhu cầu giảng dạy...</p>
                    <time datetime="2026-10-19">19/10/2026</time>
                </article>
                <div class="news-side">
                    <article class="news-card-compact">...</article>
                    <article class="news-card-compact">...</article>
                </div>
            </div>
        </div>
        <aside class="notices-column">
            <h2 class="section-heading"><a href="/thong-bao">THÔNG BÁO</a></h2>
            <ul class="notices-list">
                <li class="notice-card">
                    <a href="...">Sinh viên Thủy lợi hưởng ứng Ngày Văn hóa Việt Nam 24/11</a>
                    <time datetime="2026-11-13">13/11/2026</time>
                </li>
            </ul>
        </aside>
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.news-notices-layout {
    display: flex;
    align-items: flex-start;
    gap: 40px;
}

.news-column { flex: 2; min-width: 0; }
.notices-column { flex: 1; min-width: 0; }

.news-split {
    display: flex;
    gap: 30px;
}

.news-featured { flex: 2; }
.news-side {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.notices-list {
    display: flex;
    flex-direction: column;
    gap: 12px;
}
```

### 7. Kết luận tư duy
```text
news-notices-section
Layout : news-column (featured + 2 side) + notices-column (6 items)
CSS    : Nested Flexbox (tỷ lệ 2:1 ngoài và 2:1 trong) hoặc CSS Grid 3 cột
HTML   : section > div(h2, article) + aside(h2, ul > li > a + time)
```

---

## 06. Sự kiện (`events-section`)

### 1. Nhận diện Layout Box
```text
main-content
└── events-section
    └── container
        ├── section-heading (SỰ KIỆN)
        └── events-slider
            ├── events-track
            │   └── event-card (hiển thị 4 thẻ/hàng)
            │       ├── event-media
            │       │   ├── event-image
            │       │   └── event-date-badge
            │       └── event-title
            └── events-pagination (8 nút chấm)
```
- **Giải thích:** Danh sách sự kiện trình bày theo dạng băng chuyền ngang 4 thẻ cùng lúc; mỗi thẻ có nhãn ngày xanh đè lên góc trên bên trái của ảnh thumbnail.

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Flexbox (Carousel track) + Position (`relative`/`absolute` cho badge ngày)**.
- `.events-track` là **Flex Container** (`display: flex;`), mỗi `.event-card` là **Flex Item** (`flex: 0 0 25%;` trên desktop để hiển thị đúng 4 thẻ vừa khung `100%`).
- `.event-media` đặt `position: relative;` làm mốc cho `.event-date-badge` (`position: absolute; top: 10px; left: 10px;`).

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Tiêu đề `SỰ KIỆN` | Tiêu đề section | `h2 > a` |
| Mỗi thẻ sự kiện | Thẻ nội dung độc lập | `article` |
| Ảnh sự kiện | Ảnh minh họa 16:9 | `img` |
| Nhãn ngày `6 / THÁNG 10` | Thời gian diễn ra | `time` |
| Tên sự kiện | Tiêu đề + liên kết | `h3 > a` |
| 8 nút chấm phân trang | Điều khiển chuyển trang | `button` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="events-section">
    <div class="container">
        <div class="section-heading">SỰ KIỆN</div>
        <div class="events-slider">
            <div class="events-track">
                <div class="event-card">
                    <div class="event-media">
                        <div class="event-image"></div>
                        <div class="event-date-badge">6 / THÁNG 10</div>
                    </div>
                    <div class="event-title">Hưởng ứng Ngày Chuyển đổi số...</div>
                </div>
            </div>
            <div class="events-pagination"></div>
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="events-section">
    <div class="container">
        <h2 class="section-heading"><a href="/su-kien">SỰ KIỆN</a></h2>
        <div class="events-slider">
            <div class="events-track">
                <article class="event-card">
                    <div class="event-media">
                        <img src="event-1.jpg" alt="...">
                        <time class="event-date-badge" datetime="2026-10-06">6 / THÁNG 10</time>
                    </div>
                    <h3><a href="...">Hưởng ứng Ngày Chuyển đổi số quốc gia 10/10/2026</a></h3>
                </article>
            </div>
            <div class="events-pagination" aria-label="Các trang sự kiện">
                <button type="button" aria-label="Hiện trang sự kiện 1"></button>
            </div>
        </div>
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.events-slider {
    overflow: hidden;
}

.events-track {
    display: flex;
}

.event-card {
    flex: 0 0 25%;
    padding: 0 10px;
}

.event-media {
    position: relative;
}

.event-date-badge {
    position: absolute;
    top: 10px;
    left: 10px;
}

.events-pagination {
    display: flex;
    justify-content: center;
    gap: 8px;
}
```

### 7. Kết luận tư duy
```text
events-section
Layout : section-heading + events-track (4 cards/view) + pagination
CSS    : Flexbox row (flex: 0 0 25%) + Position relative/absolute
HTML   : section > h2 + article(img, time, h3 > a) + button
```

---

## 07. Những con số ấn tượng (`statistics-section`)

### 1. Nhận diện Layout Box
```text
main-content
└── statistics-section
    └── container
        ├── section-heading (NHỮNG CON SỐ ẤN TƯỢNG)
        └── stats-list
            └── stat-item (x5)
                ├── stat-value (24.000+, 500+, 95%, 75+, 60%)
                └── stat-label (Người học, Đối tác...)
```

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Flexbox** (hoặc Grid 5 cột đều).
- `.stats-list`: Flex Container (`display: flex; flex-wrap: wrap; justify-content: space-between;`), mỗi `.stat-item` chiếm `flex: 1 1 20%;` trên desktop.
- Bên trong `.stat-item`: Flex Container dọc (`display: flex; flex-direction: column; align-items: center; text-align: center;`).
- **Điểm hay thực tế đã quan sát:** Trang dùng `<dl>` với `order: 1` cho `<dd>` (số to xanh) và `order: 2` cho `<dt>` (nhãn chữ), giúp giữ đúng ngữ nghĩa HTML (`<dt>` định nghĩa thuật ngữ trước `<dd>` giá trị) mà vẫn hiển thị số to ở trên.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| `NHỮNG CON SỐ ẤN TƯỢNG` | Tiêu đề khối | `h2` |
| Danh sách 5 chỉ số cặp nhãn–giá trị | Bảng dữ liệu mô tả | `dl` |
| Nhãn (`Người học`, `Ngành học`...) | Tên đại lượng | `dt` |
| Con số (`24.000+`, `95%`...) | Giá trị thống kê | `dd` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="statistics-section">
    <div class="container">
        <div class="section-heading">NHỮNG CON SỐ ẤN TƯỢNG</div>
        <div class="stats-list">
            <div class="stat-item">
                <div class="stat-value">24.000+</div>
                <div class="stat-label">Người học</div>
            </div>
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="statistics-section">
    <div class="container">
        <h2 class="section-heading">NHỮNG CON SỐ ẤN TƯỢNG</h2>
        <dl class="stats-list">
            <div class="stat-item">
                <dt class="stat-label">Người học</dt>
                <dd class="stat-value">24.000+</dd>
            </div>
            <!-- lặp 5 cặp -->
        </dl>
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.stats-list {
    display: flex;
    flex-wrap: wrap;
}

.stat-item {
    flex: 1 1 20%;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
}

.stat-value { order: 1; }
.stat-label { order: 2; }
```

### 7. Kết luận tư duy
```text
statistics-section
Layout : heading + 5 stat-item (value + label)
CSS    : Flexbox row + inner Flexbox column (order 1 / order 2)
HTML   : section > h2 + dl > div(dt + dd)
```

---

## 08. Banner nổi bật (`highlight-banner`)

### 1. Nhận diện Layout Box
```text
main-content
└── highlight-banner
    └── banner-image
```
- **Giải thích:** Khối ảnh rộng tràn 100% chiều ngang màn hình (`aspect-ratio: 1020 / 392`, nền `#0118d8`), không chứa cột hay chữ DOM đè lên.

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Normal Flow** (khối block bình thường với `width: 100%` và `aspect-ratio`).
- > **Không cần Flexbox/Grid** vì bên trong chỉ có một hình ảnh đơn lẻ trải tràn hết khung.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Vùng banner | Phần nội dung trang | `section` |
| Hình ảnh truyền thông | Nội dung trực quan chính | `figure`, `img` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="highlight-banner">
    <div class="banner-image"></div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="highlight-banner" aria-label="Thông điệp nổi bật">
    <figure class="banner-media">
        <img src="GS-Nguyen-Trung-Viet.webp" alt="Trường Đại học Thủy lợi" loading="lazy">
    </figure>
</section>
```

### 6. CSS Layout minh họa
```css
.highlight-banner {
    width: 100%;
    aspect-ratio: 1020 / 392;
    overflow: hidden;
}

.banner-media img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
}
```

### 7. Kết luận tư duy
```text
highlight-banner
Layout : single full-bleed media box
CSS    : Normal Flow (block + aspect-ratio + object-fit: cover)
HTML   : section > figure > img
```

---

## 09. Tuyển sinh (`admission-section`)

### 1. Nhận diện Layout Box
```text
main-content
└── admission-section
    └── container (admission-layout)
        ├── admission-media
        └── admission-content
            ├── section-heading (TUYỂN SINH)
            ├── admission-desc
            ├── admission-levels (3 bậc: Đại học, Thạc sĩ, Tiến sĩ)
            └── admission-cta (ĐĂNG KÝ NGAY)
```

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Flexbox**.
- `.admission-layout`: Flex Container ngang (`display: flex; align-items: center; gap: 40px;`), chia đôi **tỷ lệ 1 : 1** (`flex: 1` cho ảnh trái và `flex: 1` cho nội dung phải).
- `.admission-content`: Flex Container dọc (`display: flex; flex-direction: column; align-items: flex-start; gap: 16px;`) để căn trái gọn gàng và giữ nút `ĐĂNG KÝ NGAY` đúng kích thước nội dung chứ không giãn hết chiều ngang.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Ảnh tuyển sinh (`4:3`) | Minh họa | `img` |
| Tiêu đề `TUYỂN SINH` | Tiêu đề mục + link | `h2 > a` |
| Đoạn giới thiệu | Mô tả ngắn | `p` |
| 3 bậc (`Đại học`, `Thạc sĩ`, `Tiến sĩ`) | Danh sách đường dẫn | `ul > li > a` |
| Nút `ĐĂNG KÝ NGAY` | Liên kết CTA tới cổng tuyển sinh | `a` (tạo dáng nút) |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="admission-section">
    <div class="container admission-layout">
        <div class="admission-media"></div>
        <div class="admission-content">
            <div class="section-heading">TUYỂN SINH</div>
            <div class="admission-desc"></div>
            <div class="admission-levels">
                <div class="level-item">Đại học</div>
                <div class="level-item">Thạc sĩ</div>
                <div class="level-item">Tiến sĩ</div>
            </div>
            <div class="admission-cta">ĐĂNG KÝ NGAY</div>
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="admission-section">
    <div class="container admission-layout">
        <div class="admission-media">
            <img src="tuyen-sinh.jpg" alt="Tuyển sinh Trường Đại học Thủy lợi">
        </div>
        <div class="admission-content">
            <h2><a href="https://ts.tlu.edu.vn/">TUYỂN SINH</a></h2>
            <p>Cung cấp thông tin tuyển sinh, ngành tuyển sinh, đề án tuyển sinh...</p>
            <ul class="admission-levels">
                <li><a href="...">Đại học</a></li>
                <li><a href="...">Thạc sĩ</a></li>
                <li><a href="...">Tiến sĩ</a></li>
            </ul>
            <a href="https://ts.tlu.edu.vn/" class="btn-cta">ĐĂNG KÝ NGAY</a>
        </div>
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.admission-layout {
    display: flex;
    align-items: center;
    gap: 40px;
}

.admission-media,
.admission-content {
    flex: 1 1 0;
}

.admission-content {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 16px;
}
```

### 7. Kết luận tư duy
```text
admission-section
Layout : media (50%) + content (50%)
CSS    : Flexbox row (1:1, align-items: center) + inner Flexbox column
HTML   : section > div(img) + div(h2, p, ul > li > a, a.btn)
```

---

## 10. Bộ ba Đào tạo / Sinh viên / Cựu sinh viên (`triad-section`)

### 1. Nhận diện Layout Box
```text
main-content
└── triad-section
    └── container (triad-layout)
        ├── triad-column (ĐÀO TẠO)
        │   ├── section-heading
        │   └── feature-card (ảnh + tiêu đề + trích dẫn)
        ├── triad-column (SINH VIÊN)
        └── triad-column (CỰU SINH VIÊN)
```

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Flexbox** (hoặc CSS Grid 3 cột `repeat(3, 1fr)` như thực tế trang đang dùng).
- Với Flexbox: `.triad-layout` là Flex Container ngang (`display: flex; gap: 24px;`), 3 `.triad-column` dùng `flex: 1 1 0; min-width: 0; display: flex; flex-direction: column;` và `.feature-card` đặt `flex: 1;` để 3 thẻ có chiều cao đáy bằng nhau tuyệt đối.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Tiêu đề mỗi cột (`ĐÀO TẠO`, `SINH VIÊN`, `CỰU SINH VIÊN`) | Tiêu đề chuyên mục | `h2 > a` |
| Thẻ bài viết trong mỗi cột | Bài viết độc lập | `article` |
| Ảnh 16:9, tiêu đề bài, đoạn tóm tắt | Nội dung bài | `img`, `h3 > a`, `p` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="triad-section">
    <div class="container triad-layout">
        <div class="triad-column">
            <div class="section-heading">ĐÀO TẠO</div>
            <div class="feature-card">
                <div class="card-thumb"></div>
                <div class="card-title"></div>
                <div class="card-excerpt"></div>
            </div>
        </div>
        <!-- lặp cho SINH VIÊN và CỰU SINH VIÊN -->
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="triad-section" aria-label="Đào tạo, Sinh viên và Cựu sinh viên">
    <div class="container triad-layout">
        <div class="triad-column">
            <h2 class="section-heading"><a href="/dao-tao">ĐÀO TẠO</a></h2>
            <article class="feature-card">
                <img src="dao-tao.jpg" alt="...">
                <h3><a href="...">Thông tin luận án và thời gian bảo vệ luận án tiến sĩ...</a></h3>
                <p>Trường Đại học Thủy lợi tổ chức bảo vệ luận án tiến sĩ...</p>
            </article>
        </div>
        <!-- lặp cho SINH VIÊN và CỰU SINH VIÊN -->
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.triad-layout {
    display: flex;
    gap: 24px;
}

.triad-column {
    flex: 1 1 0;
    min-width: 0;
    display: flex;
    flex-direction: column;
}

.feature-card {
    flex: 1;
}
```

### 7. Kết luận tư duy
```text
triad-section
Layout : 3 triad-column bằng nhau (mỗi cột = heading + feature-card)
CSS    : Flexbox row (flex: 1, min-width: 0) hoặc CSS Grid 3 cột
HTML   : section > 3 × div(h2 > a + article(img, h3 > a, p))
```

---

## 11. Các đơn vị đào tạo, KHCN (`units-section`)

### 1. Nhận diện Layout Box
```text
main-content
└── units-section (nền xanh + ảnh nền mờ)
    └── container (units-layout)
        ├── units-left
        │   ├── section-heading (CÁC ĐƠN VỊ ĐÀO TẠO, KHCN)
        │   └── units-vertical-slider (11 đơn vị + fade trên/dưới)
        └── units-right
            ├── unit-description
            └── unit-cta-pill ("Khám phá Khoa Công trình >")
```

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Flexbox (2 cột trái–phải) + Position (`relative`/`absolute` cho gradient fade và lớp ảnh nền)**.
- `.units-layout`: `display: flex; gap: 40px; min-height: 500px;`.
- `.units-left`: `flex: 1; display: flex; flex-direction: column; justify-content: center;`.
- `.units-right`: `flex: 1; display: flex; flex-direction: column; justify-content: space-between; align-items: flex-start;` (đẩy mô tả lên trên và nút pill xuống dưới).

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| `CÁC ĐƠN VỊ ĐÀO TẠO, KHCN` | Tiêu đề mục | `h2` |
| Danh sách 11 Khoa / Phân hiệu / Trung tâm | Điều khiển chuyển tab dọc | `ul > li > button` (hoặc `role="tablist"`) |
| Đoạn mô tả đơn vị đang chọn | Nội dung động | `p` |
| Nút `Khám phá <Tên đơn vị> >` | Chuyển tới trang khoa | `a` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="units-section">
    <div class="container units-layout">
        <div class="units-left">
            <div class="section-heading">CÁC ĐƠN VỊ ĐÀO TẠO, KHCN</div>
            <div class="units-vertical-slider">
                <div class="unit-name">KHOA ĐIỆN - ĐIỆN TỬ</div>
                <div class="unit-name is-active">KHOA CÔNG TRÌNH</div>
                <div class="unit-name">KHOA KỸ THUẬT TÀI NGUYÊN NƯỚC</div>
            </div>
        </div>
        <div class="units-right">
            <div class="unit-description"></div>
            <div class="unit-cta-pill">Khám phá Khoa Công trình ></div>
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="units-section">
    <div class="container units-layout">
        <div class="units-left">
            <h2>CÁC ĐƠN VỊ ĐÀO TẠO, KHCN</h2>
            <ul class="units-vertical-slider" role="tablist">
                <li><button type="button" role="tab">KHOA ĐIỆN - ĐIỆN TỬ</button></li>
                <li><button type="button" role="tab" aria-selected="true">KHOA CÔNG TRÌNH</button></li>
            </ul>
        </div>
        <div class="units-right" role="tabpanel">
            <p>Khoa Công trình là một trong những đơn vị đào tạo và nghiên cứu hàng đầu...</p>
            <a href="/khoa-cong-trinh" class="unit-cta-pill">Khám phá Khoa Công trình &rsaquo;</a>
        </div>
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.units-section {
    position: relative;
}

.units-layout {
    display: flex;
    align-items: stretch;
    gap: 40px;
    min-height: 500px;
}

.units-left,
.units-right {
    flex: 1;
    display: flex;
    flex-direction: column;
}

.units-right {
    justify-content: space-between;
    align-items: flex-start;
}
```

### 7. Kết luận tư duy
```text
units-section
Layout : units-left (heading + 11 units list) + units-right (desc + pill CTA)
CSS    : Flexbox row (1:1) + inner Flexbox column (space-between)
HTML   : section > div(h2, ul > li > button) + div(p, a)
```

---

## 12. Khối tin 3+3 lặp lại: Hợp tác quốc tế / Nghiên cứu / Vì cộng đồng (`showcase-section`)

> **Mẫu tái sử dụng:** Cả 3 section trên trang (`HỢP TÁC QUỐC TẾ`, `NGHIÊN CỨU`, `VÌ CỘNG ĐỒNG`) dùng **chung 1 cấu trúc bố cục** gồm 3 thẻ ảnh bên trái và 1 cột 3 tin chữ bên phải.

### 1. Nhận diện Layout Box
```text
main-content
└── showcase-section (x3: Hợp tác quốc tế / Nghiên cứu / Vì cộng đồng)
    └── container
        ├── section-heading
        └── showcase-layout
            ├── showcase-cards (3 thẻ ảnh bằng nhau)
            │   └── article-card (x3)
            └── showcase-side-list (1 khung trắng chứa 3 tin chữ)
                └── text-item (x3)
```

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** Trang thực tế dùng CSS Grid (`grid-cols-[repeat(3,minmax(0,1fr))_minmax(230px,1fr)] gap-5`).
- Nếu phân tích theo **Flexbox**:
  - `.showcase-layout`: Flex Container (`display: flex; gap: 20px; align-items: stretch;`), chia tỷ lệ `flex: 3` (`.showcase-cards`) và `flex: 1; min-width: 230px;` (`.showcase-side-list`).
  - `.showcase-cards`: Flex Container con (`display: flex; gap: 20px;`), 3 thẻ mỗi thẻ `flex: 1 1 0; min-width: 0;`.
  - `.showcase-side-list`: Flex Container dọc (`display: flex; flex-direction: column; justify-content: space-between;`) để 3 tin vắn chia đều chiều cao khung bên phải.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Tiêu đề section | Tiêu đề mục + link | `h2 > a` |
| 3 thẻ tin có ảnh | Bài viết độc lập | `article` (`img`, `span`, `h3 > a`, `time`) |
| 3 tin vắn bên phải | Danh sách tin bổ sung không ảnh | `ul > li` (`span`, `h3 > a`, `time`) |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="showcase-section">
    <div class="container">
        <div class="section-heading">HỢP TÁC QUỐC TẾ</div>
        <div class="showcase-layout">
            <div class="showcase-cards">
                <div class="article-card">...</div> <!-- x3 -->
            </div>
            <div class="showcase-side-list">
                <div class="text-item">...</div> <!-- x3 -->
            </div>
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="showcase-section">
    <div class="container">
        <h2 class="section-heading"><a href="/doi-ngoai">HỢP TÁC QUỐC TẾ</a></h2>
        <div class="showcase-layout">
            <div class="showcase-cards">
                <article class="article-card">
                    <img src="coop-1.jpg" alt="...">
                    <span>HỢP TÁC QUỐC TẾ</span>
                    <h3><a href="...">Thứ trưởng Bộ Thủy lợi Trung Quốc...</a></h3>
                    <time datetime="2026-11-19">19/11/2026</time>
                </article>
            </div>
            <ul class="showcase-side-list">
                <li class="text-item">
                    <span>HỢP TÁC QUỐC TẾ</span>
                    <h3><a href="...">Trường ĐH Thủy lợi tăng cường hợp tác...</a></h3>
                    <time datetime="2026-11-16">16/11/2026</time>
                </li>
            </ul>
        </div>
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.showcase-layout {
    display: flex;
    align-items: stretch;
    gap: 20px;
}

.showcase-cards {
    flex: 3;
    display: flex;
    gap: 20px;
    min-width: 0;
}

.article-card {
    flex: 1 1 0;
    min-width: 0;
}

.showcase-side-list {
    flex: 1;
    min-width: 230px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}
```

### 7. Kết luận tư duy
```text
showcase-section (x3)
Layout : 3 article-card + 1 side-list (3 text-item)
CSS    : Nested Flexbox (tỷ lệ 3:1) hoặc CSS Grid 4 cột
HTML   : section > h2 + div(3 × article) + ul(3 × li)
```

---

## 13. Mạng lưới đối tác (`partners-section`)

### 1. Nhận diện Layout Box
```text
main-content
└── partners-section (nền xanh #0118d8)
    └── container (partners-layout)
        ├── section-heading (MẠNG LƯỚI ĐỐI TÁC)
        ├── partners-desc
        └── partners-strip (dải nền trắng)
            └── partners-track (23 logo)
```

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **Flexbox (căn dọc khối chữ + track ngang logo)**.
- `.partners-layout`: `display: flex; flex-direction: column; align-items: center; text-align: center; gap: 16px;`.
- `.partners-track`: `display: flex; align-items: center;` giúp các logo (tròn, chữ nhật ngang, dọc) luôn được căn thẳng tâm theo chiều dọc; mỗi logo item chiếm `flex: 0 0 calc(100% / 6);` (~6 logo/khung trên desktop).

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| `MẠNG LƯỚI ĐỐI TÁC` | Tiêu đề mục | `h2` |
| Đoạn văn giới thiệu | Mô tả mạng lưới | `p` |
| 23 logo đại học / tổ chức | Danh sách hình ảnh đối tác | `ul > li > img` (bọc `a` nếu bấm được) |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="partners-section">
    <div class="container partners-layout">
        <div class="section-heading">MẠNG LƯỚI ĐỐI TÁC</div>
        <div class="partners-desc"></div>
        <div class="partners-strip">
            <div class="partners-track">
                <div class="partner-logo"></div> <!-- x23 -->
            </div>
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="partners-section">
    <div class="container partners-layout">
        <h2>MẠNG LƯỚI ĐỐI TÁC</h2>
        <p>Trường Đại học Thủy lợi tự hào là đối tác của các đại học, viện nghiên cứu...</p>
        <div class="partners-strip">
            <ul class="partners-track">
                <li class="partner-logo"><img src="kgu.png" alt="Kanto Gakuin University"></li>
                <li class="partner-logo"><img src="kyushu.png" alt="Kyushu University"></li>
            </ul>
        </div>
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.partners-layout {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
}

.partners-strip {
    width: 100%;
    overflow: hidden;
}

.partners-track {
    display: flex;
    align-items: center;
}

.partner-logo {
    flex: 0 0 16.666%;
    display: flex;
    justify-content: center;
}
```

### 7. Kết luận tư duy
```text
partners-section
Layout : heading + desc + white strip (partners-track)
CSS    : Flexbox column (căn giữa) + Flexbox row (align-items: center)
HTML   : section > h2 + p + div > ul > li > img
```

---

## 14. Khoảnh khắc TLU (`moments-section`)

### 1. Nhận diện Layout Box
```text
main-content
└── moments-section
    └── container
        ├── section-heading (KHOẢNH KHẮC TLU)
        └── moments-bento (5 cột × 4 hàng = 15 ảnh)
            ├── moment-tile (hàng 1: 5 ô 1×1)
            ├── moment-tile (trái hàng 2–3: 2 ô 1×1)
            ├── moment-tile--featured (giữa: 1 ô lớn 3×2)
            ├── moment-tile (phải hàng 2–3: 2 ô 1×1)
            └── moment-tile (hàng 4: 5 ô 1×1)
```

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **CSS Grid là tối ưu nhất** (thực tế trang đang dùng `grid-cols-5 auto-rows-[160px]` và ô trung tâm `col-span-3 row-span-2`).
- **Tại sao Grid phù hợp hơn Flexbox?** Vì ảnh trung tâm **gộp cả 3 cột theo chiều ngang lẫn 2 hàng theo chiều dọc** (`3×2`). Flexbox là công cụ 1 chiều, muốn làm được bố cục này phải tách DOM thành 3 hàng và lồng thêm cột phụ hai bên (như đã phác thảo ở Phần 1), gây phức tạp khi đổi số cột trên tablet/mobile.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| `KHOẢNH KHẮC TLU` | Tiêu đề bộ sưu tập | `h2` |
| 15 bức ảnh hoạt động | Bộ sưu tập hình ảnh | `ul > li > figure > img` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="moments-section">
    <div class="container">
        <div class="section-heading">KHOẢNH KHẮC TLU</div>
        <div class="moments-bento">
            <div class="moment-tile"></div> <!-- 0..5 -->
            <div class="moment-tile moment-tile--featured"></div> <!-- ô số 6: 3x2 -->
            <div class="moment-tile"></div> <!-- 7..14 -->
        </div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<section class="moments-section">
    <div class="container">
        <h2 class="section-heading">KHOẢNH KHẮC TLU</h2>
        <ul class="moments-bento">
            <li class="moment-tile"><figure><img src="m1.jpg" alt="..." loading="lazy"></figure></li>
            <!-- ... -->
            <li class="moment-tile moment-tile--featured"><figure><img src="m7.jpg" alt="..." loading="lazy"></figure></li>
            <!-- ... -->
        </ul>
    </div>
</section>
```

### 6. CSS Layout minh họa
```css
.moments-bento {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-auto-rows: 160px;
    gap: 12px;
}

.moment-tile--featured {
    grid-column: 2 / span 3;
    grid-row: 2 / span 2;
}
```

### 7. Kết luận tư duy
```text
moments-section
Layout : heading + Bento 15 ảnh (1 ô trung tâm 3×2)
CSS    : CSS Grid 2D (5 cột × auto-rows 160px)
HTML   : section > h2 + ul > li > figure > img
```

---

## 15. Footer (`site-footer`)

### 1. Nhận diện Layout Box
```text
page-wrapper
└── site-footer (nền xanh đậm #1907dc)
    └── container (footer-inner)
        ├── footer-top-band (4 cột: Logo + Địa chỉ + Email + Điện thoại)
        ├── footer-middle-band (4 cột: Giới thiệu | Truy cập nhanh | Theo dõi TLU | Video)
        └── footer-bottom-band (dòng bản quyền)
```

### 2. Tư duy dàn trang CSS
- **Layout mode chính:** **CSS Grid** (thực tế trang dùng `lg:grid-cols-[1.35fr_1fr_1fr_1fr]` ở tầng trên và `lg:grid-cols-4` ở tầng giữa để 4 cột trên–dưới thẳng hàng), hoặc **Flexbox**:
  - `.footer-top-band`: `display: flex; align-items: center; gap: 48px;` (Logo `flex: 1.35`, 3 khối liên hệ mỗi khối `flex: 1`).
  - `.footer-middle-band`: `display: flex; gap: 48px;` (4 cột `flex: 1 1 0;`).
  - `.footer-bottom-band`: **Normal Flow** (`text-align: center; padding-top: 40px;`) — **không cần Flexbox**.

### 3. Nhận diện nội dung bên trong
| Nội dung | Vai trò | HTML phù hợp |
|---|---|---|
| Logo trắng ĐẠI HỌC THỦY LỢI | Nhận diện cuối trang | `img` |
| Địa chỉ (`175 Tây Sơn...`), Email, SĐT | Thông tin liên hệ | `address`, `a[href^="mailto:"]`, `a[href^="tel:"]` |
| `GIỚI THIỆU`, `TRUY CẬP NHANH`, `THEO DÕI TLU` | Điều hướng chân trang | `nav`, `h3`, `ul > li > a` |
| Khung video `TRƯỜNG ĐẠI HỌC THỦY LỢI` | Video giới thiệu trường | `video` (hoặc `iframe`) |
| `Bản quyền © 2026...` | Thông tin pháp lý | `p`, `small`, `a` |

### 4. Phiên bản chỉ dùng `div`
```html
<div class="site-footer">
    <div class="container">
        <div class="footer-top-band">
            <div class="footer-brand"></div>
            <div class="footer-contact">Địa chỉ...</div>
            <div class="footer-contact">Email...</div>
            <div class="footer-contact">Điện thoại...</div>
        </div>
        <div class="footer-middle-band">
            <div class="footer-col">GIỚI THIỆU</div>
            <div class="footer-col">TRUY CẬP NHANH</div>
            <div class="footer-col">THEO DÕI TLU</div>
            <div class="footer-col">VIDEO</div>
        </div>
        <div class="footer-bottom-band">Bản quyền © 2026...</div>
    </div>
</div>
```

### 5. Phiên bản HTML Semantic
```html
<footer class="site-footer">
    <div class="container">
        <div class="footer-top-band">
            <img src="logo-white.png" alt="Trường Đại học Thủy lợi">
            <address>Địa chỉ: 175 Tây Sơn - P. Kim Liên - Hà Nội</address>
            <div>Email: <a href="mailto:daihocthuyloi@tlu.edu.vn">daihocthuyloi@tlu.edu.vn</a></div>
            <div>Điện thoại: <a href="tel:02438522201">(024) 38522201</a></div>
        </div>
        <div class="footer-middle-band">
            <nav aria-label="Giới thiệu"><h3>GIỚI THIỆU</h3><ul>...</ul></nav>
            <nav aria-label="Truy cập nhanh"><h3>TRUY CẬP NHANH</h3><ul>...</ul></nav>
            <nav aria-label="Theo dõi TLU"><h3>THEO DÕI TLU</h3><ul>...</ul></nav>
            <div class="footer-media">
                <h3>TRƯỜNG ĐẠI HỌC THỦY LỢI</h3>
                <video controls src="1.mp4"></video>
            </div>
        </div>
        <p class="footer-bottom-band"><small>Bản quyền © 2026 Trường Đại học Thủy lợi...</small></p>
    </div>
</footer>
```

### 6. CSS Layout minh họa
```css
.footer-top-band {
    display: flex;
    align-items: center;
    gap: 48px;
}

.footer-brand { flex: 1.35; }
.footer-contact-item { flex: 1; }

.footer-middle-band {
    display: flex;
    gap: 48px;
}

.footer-col {
    flex: 1 1 0;
}

.footer-bottom-band {
    text-align: center;
    padding-top: 40px;
}
```

### 7. Kết luận tư duy
```text
site-footer
Layout : footer-top (1.35:1:1:1) + footer-middle (4 cols) + footer-bottom
CSS    : Flexbox/Grid cho 2 tầng trên + Normal Flow căn giữa cho tầng đáy
HTML   : footer > address + a(mailto/tel) + 3 × nav(h3, ul > li > a) + video + p(small)
```

---

# BƯỚC 3 — BẢN ĐỒ FLEXBOX TRÊN TOÀN TRANG

```text
PAGE-WRAPPER                                  [FLEX: column, min-height: 100vh]
│
├── HEADER
│   ├── header-top__inner                     [FLEX: row, space-between, center]
│   │   └── header-controls                   [NESTED FLEX: row, gap 12px]
│   │       └── search-form                   [NESTED FLEX: row, input flex: 1]
│   └── nav-list                              [FLEX: row, stretch]
│       └── nav-item (x8)                     [FLEX ITEM: flex: auto + NESTED FLEX center]
│
├── FLOATING-SOCIAL                           [FIXED + FLEX: column, gap 8px]
│
├── MAIN-CONTENT                              [FLEX ITEM: flex: 1]
│   ├── hero-track                            [FLEX: row, nowrap, slide flex: 0 0 100%]
│   ├── news-notices-layout                   [FLEX: row, tỷ lệ 2:1] (hoặc Grid 3 cột)
│   │   ├── news-split                        [NESTED FLEX: row, tỷ lệ 2:1]
│   │   └── notices-list                      [FLEX: column, gap 12px]
│   ├── events-track                          [FLEX: row, nowrap, card flex: 0 0 25%]
│   ├── statistics-list                       [FLEX + WRAP: row, 20%/mục]
│   │   └── stat-item                         [NESTED FLEX: column, order 1 / 2]
│   ├── highlight-banner                      [KHÔNG FLEX — Normal Flow 100%]
│   ├── admission-layout                      [FLEX: row, tỷ lệ 1:1, align center]
│   │   └── admission-content                 [NESTED FLEX: column, align flex-start]
│   ├── triad-layout                          [FLEX: row, 1:1:1, min-width: 0]
│   ├── units-layout                          [FLEX: row, 1:1]
│   │   └── units-right                       [NESTED FLEX: column, space-between]
│   ├── showcase-layout (×3)                  [FLEX: row, tỷ lệ 3:1]
│   │   ├── showcase-cards                    [NESTED FLEX: row, 1:1:1]
│   │   └── showcase-side-list                [NESTED FLEX: column, space-between]
│   ├── partners-layout                       [FLEX: column, align center]
│   │   └── partners-track                    [FLEX: row, align center]
│   └── moments-bento                         [KHÔNG FLEX — CSS Grid 2D]
│
└── FOOTER
    ├── footer-top-band                       [FLEX: row, tỷ lệ 1.35:1:1:1]
    ├── footer-middle-band                    [FLEX: row, 1:1:1:1]
    └── footer-bottom-band                    [KHÔNG FLEX — Normal Flow text-align center]
```

**Nơi KHÔNG cần Flexbox:**
- Khối cha bọc toàn trang của các `.section` xếp từ trên xuống dưới (Block flow tự nhiên).
- `highlight-banner` (chỉ có 1 thẻ ảnh tràn viền).
- Các khối chữ nội bộ như `h2` + `p` xếp dọc bình thường khi không cần `space-between` hay `order`.
- `footer-bottom-band` (chỉ 1 dòng chữ căn giữa).

---

# BƯỚC 4 — NHẬN DIỆN CSS GRID TRÊN TOÀN TRANG

Dù nhiều hàng cột trên trang có thể mô phỏng bằng Flexbox, **CSS Grid phù hợp hơn hẳn Flexbox** tại 4 vùng sau (và thực tế code của `tlu.edu.vn` cũng chọn Grid cho các vùng này):

1. **Khoảnh khắc TLU (`moments-bento` — 5 cột × 4 hàng, 1 ô trung tâm `3×2`):**
   - **Flexbox dùng được không?** Rất gượng gạo (phải chia 3 hàng riêng và lồng thêm cột phụ hai bên, khó đổi layout khi responsive).
   - **Grid phù hợp hơn:** Đặt `grid-template-columns: repeat(5, 1fr); grid-auto-rows: 160px;` và chỉ cần 1 quy tắc `grid-column: span 3; grid-row: span 2;` cho ảnh trung tâm.

2. **Tin tức & Thông báo + Các khối Showcase `3+3` (`Hợp tác QT`, `Nghiên cứu`, `Vì cộng đồng`):**
   - **Grid phù hợp hơn** vì các cột thẻ tin cần căn thẳng mép trên–dưới theo **lưới cột cố định** (`repeat(3, minmax(0, 1fr)) minmax(230px, 1fr)`) mà không sợ nội dung dài làm lệch chiều rộng cột.

3. **Đào tạo / Sinh viên / Cựu sinh viên (`triad-layout`) & Những con số ấn tượng (`statistics-list`):**
   - Dùng `grid-template-columns: repeat(3, 1fr)` hoặc `repeat(5, 1fr)` giúp các cột luôn đều nhau tuyệt đối và chuyển từ `5 → 2` cột (hoặc `3 → 1` cột) chỉ bằng 1 dòng media query mà không cần tính lại `flex-basis` trừ `gap`.

4. **Footer (Tầng 1 & Tầng 2):**
   - Dùng Grid `1.35fr 1fr 1fr 1fr` và `repeat(4, 1fr)` đảm bảo cột `Địa chỉ`, `Email`, `Điện thoại` ở tầng trên thẳng hàng dọc tuyệt đối với các cột `TRUY CẬP NHANH`, `THEO DÕI TLU`, `VIDEO` ở tầng dưới.

---

# BƯỚC 5 — NHẬN DIỆN HÀNH VI RESPONSIVE (DỰ KIẾN & QUAN SÁT)

*(Quan sát trực tiếp: tại `862px`, `main-nav` ẩn đi và hiện nút `"Mở menu"` trên `header-top`; `floating-social` ẩn ở màn hình nhỏ `< 768px`. Hành vi các section dưới đây kết hợp quan sát responsive classes thực tế trên trang.)*

| Khu vực | Desktop (`>= 1025px`) | Tablet (`768px – 1024px`) | Mobile (`< 768px`) | Cách xử lý CSS |
|---|---|---|---|---|
| **Header & Nav** | Logo + Search + Lang; Nav 8 mục ngang | Ẩn thanh Nav xanh, hiện nút Hamburger | Search thu gọn / xuống dòng, giữ Logo + Menu btn | Media query ẩn/hiện tại `1025px` |
| **Tin tức & Thông báo** | `Tin tức (2fr + 1fr)` \| `Thông báo` | Tin tức xếp trên, Thông báo xuống dưới | Tất cả xếp 1 cột dọc | `flex-direction: column` hoặc `grid-template-columns: 1fr` |
| **Sự kiện** | `A \| B \| C \| D` (4 thẻ) | `A \| B` (2 thẻ/khung) | `A` (1 thẻ/khung) | Đổi `flex: 0 0 25%` → `50%` → `100%` |
| **Con số ấn tượng** | `1 \| 2 \| 3 \| 4 \| 5` (5 cột) | `1 \| 2 \| 3 \| 4 \| 5` (`>=850px`) | `1 \| 2` / `3 \| 4` / `5` (2 cột) | `grid-cols-2` → `min-[850px]:grid-cols-5` |
| **Tuyển sinh & Đơn vị** | `Trái \| Phải` (2 cột 50/50) | `Trái \| Phải` | `Trái` trên, `Phải` dưới | `flex-direction: column` hoặc `md:grid-cols-2` |
| **Bộ ba & Showcase 3+3** | `A \| B \| C` hoặc `A \| B \| C \| List` | Xếp lại gọn hoặc 3+1 | `A` / `B` / `C` / `List` (1 cột) | `md:grid-cols-3` / `flex-direction: column` |
| **Khoảnh khắc TLU** | 5 cột (ô lớn `3×2`) | 4 cột (`>=768px`) | 3 cột (`auto-rows: 100px`) | Thay đổi `grid-template-columns` & `auto-rows` theo breakpoint |
| **Footer** | 4 cột ngang đều 2 tầng | 2 cột × 2 hàng (`sm:grid-cols-2`) | 1 cột xếp dọc | `grid-cols-1` → `sm:grid-cols-2` → `lg:grid-cols-4` |

---

# BƯỚC 6 — CÂY DOM GIẢ LẬP TOÀN TRANG

## 6A. DOM chỉ dùng `div` (Tư duy khoanh hộp)
```text
div.page-wrapper
├── div.header
│   ├── div.header-top > div.container > (div.brand-logo + div.header-controls)
│   └── div.main-nav > div.container > div.nav-list > 8 × div.nav-item
├── div.floating-social > 4 × div.social-btn
├── div.main-content
│   ├── div.hero-slider > div.hero-track + div.hero-pagination
│   ├── div.news-notices > div.container > (div.news-column + div.notices-column)
│   ├── div.events-section > div.container > div.events-slider
│   ├── div.statistics-section > div.container > div.stats-list > 5 × div.stat-item
│   ├── div.highlight-banner > div.banner-image
│   ├── div.admission-section > div.container > (div.admission-media + div.admission-content)
│   ├── div.triad-section > div.container > 3 × div.triad-column
│   ├── div.units-section > div.container > (div.units-left + div.units-right)
│   ├── 3 × div.showcase-section > div.container > (div.showcase-cards + div.showcase-side-list)
│   ├── div.partners-section > div.container > div.partners-strip > div.partners-track
│   └── div.moments-section > div.container > div.moments-bento > 15 × div.moment-tile
└── div.site-footer > div.container > (div.footer-top + div.footer-middle + div.footer-bottom)
```

## 6B. DOM Semantic HTML (Chuẩn ngữ nghĩa)
```text
body (div.page-wrapper)
├── header.site-header
│   ├── div.header-top > a(img) + div(form[input, button] + button)
│   └── nav.main-nav > ul > 8 × li > (a | button)
├── aside.floating-social > ul > 4 × li > a(svg)
├── main.main-content
│   ├── section.hero-slider > div > figure(picture > img) + div(7 × button)
│   ├── section.news-notices > div(h2, 3 × article) + aside(h2, ul > 6 × li)
│   ├── section.events > h2 + div(article > img + time + h3) + div(8 × button)
│   ├── section.statistics > h2 + dl > 5 × div(dt + dd)
│   ├── section.highlight-banner > figure > img
│   ├── section.admission > img + div(h2, p, ul > 3 × li > a, a.btn)
│   ├── section.triad > 3 × div(h2 + article(img, h3, p))
│   ├── section.units > div(h2, ul > 11 × li > button) + div(p, a)
│   ├── 3 × section.showcase > h2 + div(3 × article) + ul(3 × li)
│   ├── section.partners > h2 + p + div > ul > 23 × li > img
│   └── section.moments > h2 + ul > 15 × li > figure > img
└── footer.site-footer
    ├── div.footer-top > img + address + a(mailto) + a(tel)
    ├── div.footer-middle > 3 × nav(h3, ul > li > a) + div(h3, video)
    └── p.footer-bottom > small > a
```

---

# BƯỚC 7 — BẢNG TỔNG HỢP TOÀN TRANG

| Component | Layout Box | Layout Technique | Flex Container? | Flex Items | Nội dung | HTML Semantic |
|---|---|---|---|---|---|---|
| Header Top | `header-top__inner` | Flexbox | Có | `brand-logo`, `header-controls` | Logo, tìm kiếm, ngôn ngữ | `header`, `a`, `img`, `form`, `input`, `button` |
| Navigation | `nav-list` | Flexbox | Có | 8 `nav-item` | 8 danh mục chính | `nav`, `ul`, `li`, `a`, `button` |
| MXH nổi | `floating-social` | Fixed + Flexbox col | Có | 4 `social-btn` | 4 kênh MXH | `aside`, `ul`, `li`, `a`, `svg` |
| Hero Slider | `hero-track` | Flexbox + Position | Có | 7 `hero-slide` | 7 banner ảnh + dots | `section`, `figure`, `picture`, `img`, `button` |
| Tin tức & Thông báo | `news-notices-layout` | Grid / Nested Flex | Có (cấp con) | `news-column`, `notices-column` | 3 bài tin + 6 thông báo | `section`, `h2`, `article`, `aside`, `ul`, `li`, `time` |
| Sự kiện | `events-track` | Flexbox + Position | Có | `event-card` (`25%`) | 4 sự kiện/khung + ngày | `section`, `h2`, `article`, `img`, `time`, `h3`, `button` |
| Con số ấn tượng | `stats-list` | Grid / Flexbox | Có (item) | 5 `stat-item` | 5 cặp số liệu thống kê | `section`, `h2`, `dl`, `dt`, `dd` |
| Banner nổi bật | `highlight-banner` | Normal Flow | Không | - | 1 ảnh toàn chiều rộng | `section`, `figure`, `img` |
| Tuyển sinh | `admission-layout` | Flexbox / Grid 2 cột | Có | `media`, `content` | Ảnh, mô tả, 3 bậc, CTA | `section`, `img`, `h2`, `p`, `ul`, `li`, `a` |
| Đào tạo / SV / Cựu SV | `triad-layout` | Grid 3 cột / Flexbox | Có | 3 `triad-column` | 3 chuyên mục × 1 bài | `section`, `h2`, `article`, `img`, `h3`, `p` |
| Đơn vị ĐT, KHCN | `units-layout` | Flexbox + Position | Có | `units-left`, `units-right` | 11 đơn vị + mô tả + CTA | `section`, `h2`, `ul`, `button`, `p`, `a` |
| HTQT / NCKH / Cộng đồng | `showcase-layout` | Grid 4 cột / Nested Flex | Có (side) | `cards` (3) + `side-list` (1) | 3 thẻ ảnh + 3 tin chữ | `section`, `h2`, `article`, `ul`, `li`, `time` |
| Đối tác | `partners-track` | Flexbox | Có | 23 `partner-logo` | Mô tả + 23 logo đối tác | `section`, `h2`, `p`, `ul`, `li`, `img` |
| Khoảnh khắc TLU | `moments-bento` | CSS Grid 2D | Không | - | 15 ảnh Bento (`5×4`) | `section`, `h2`, `ul`, `li`, `figure`, `img` |
| Footer | `footer-top` / `middle` | Grid / Flexbox | Có | Logo, liên hệ, 4 cột | Liên hệ, links, video, bản quyền | `footer`, `address`, `nav`, `ul`, `video`, `small` |

---

# BƯỚC 8 — CHECKLIST TƯ DUY DÀNH CHO SINH VIÊN

Khi mở bất kỳ website nào (như `tlu.edu.vn`), hãy đi tuần tự 7 bước sau:

```text
[1] Nhìn tổng thể từ ngoài vào trong → Khoanh các vùng full-bleed và vùng .container
        ↓
[2] Đặt tên hộp theo VAI TRÒ (header, hero, news-card), KHÔNG đặt theo màu/kích thước
        ↓
[3] Vẽ cây cha – con trước khi viết bất kỳ dòng CSS nào
        ↓
[4] Các con cần đứng ngang trên 1 trục?
        ├── Có → Chọn Flexbox (chỉ rõ Container, Items, direction, gap, ratio)
        └── Không
             ↓
        [5] Có cấu trúc 2 chiều (hàng × cột hoặc gộp ô Bento)?
                ├── Có → Chọn CSS Grid (grid-template-columns, span)
                └── Không → Giữ Normal Flow (block xếp dọc tự nhiên)
        ↓
[6] Có phần tử đè lên ảnh/mép màn hình (badge ngày, dots, MXH nổi)?
        └── Dùng Position (relative/absolute hoặc fixed)
        ↓
[7] Nhận diện từng mẩu dữ liệu → Đổi div thành HTML Semantic (a vs button, dl/dt/dd, time, article...)
```

---

# BƯỚC 9 — NGUYÊN TẮC ĐẶT TÊN CLASS NHẤT QUÁN

Dựa trên các mẫu lặp lại của `tlu.edu.vn`, hệ thống đặt tên chuẩn theo vai trò nên tuân thủ:

- **Vùng cấp trang:** `site-header`, `main-content`, `site-footer`, `floating-social`
- **Khung bọc căn giữa:** `container` (dùng chung cho mọi section cần căn giữa `1400px`)
- **Cấu trúc Section lặp lại:** `<tên-section>-section` → `<tên-section>-layout` → `<tên-section>-item`
  - Ví dụ: `showcase-section` → `showcase-layout` → `article-card` (thay vì đặt tên `blue-box`, `box-3-cot`, `left-area`, `big-image`).
- **Biến thể trạng thái/kích thước theo vai trò:** `news-card--large`, `news-card--compact`, `moment-tile--featured`, `unit-name--active`.

---

# BƯỚC 10 — CÁC LỖI TƯ DUY PHỔ BIẾN CẦN TRÁNH

1. **Nhầm lẫn giữa `<a>` và `<button>`:**
   - Trong khối `TUYỂN SINH`, `ĐĂNG KÝ NGAY` trông giống nút bấm nhưng thực chất chuyển sang trang `ts.tlu.edu.vn` → phải dùng `<a>`, **không** dùng `<button>`. Ngược lại, `eTLU` (mở menu), các chấm chuyển slide và 11 mục chọn Khoa ở khối `CÁC ĐƠN VỊ ĐÀO TẠO, KHCN` là thao tác trên giao diện → phải dùng `<button>`.
2. **Dùng Flexbox cho mọi thứ, kể cả khối 1 phần tử hoặc xếp dọc đơn giản:**
   - `highlight-banner` chỉ có 1 ảnh, `footer-bottom-band` chỉ có 1 dòng chữ → không cần bật `display: flex`.
   - Cố dùng Flexbox cho Bento Grid `moments-bento` (có ô gộp `3×2`) sẽ làm HTML phải bọc thêm nhiều `div` thừa và khó viết CSS responsive.
3. **Quên `min-width: 0` trên Flex/Grid Item chứa chữ dài:**
   - Trong `triad-layout` (3 cột) hoặc `showcase-layout`, tiêu đề bài viết dài hoặc từ không ngắt có thể đẩy cột phình rộng làm vỡ tỷ lệ `1:1:1` nếu thiếu `min-width: 0`.
4. **Dùng `<div>` cho cặp dữ liệu thống kê (`NHỮNG CON SỐ ẤN TƯỢNG`):**
   - Dùng `<dl>`, `<dt>`, `<dd>` kết hợp `order: 1` / `order: 2` của Flexbox vừa giữ đúng ngữ nghĩa HTML cho trình đọc màn hình, vừa hiển thị đúng thiết kế số to phía trên nhãn.
5. **Đặt tên class theo hình thức hiển thị thay vì ngữ nghĩa:**
   - Đặt `.blue-section` cho `MẠNG LƯỚI ĐỐI TÁC` hay `.big-left-box` cho bài tin `HOT` sẽ gây rối khi đổi giao diện hoặc đọc lại cấu trúc HTML. Hãy đặt `.partners-section` và `.news-featured`.
