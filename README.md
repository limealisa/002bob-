# Setup Tutorial / Hướng Dẫn Cài Đặt (搭建教程)

## 1. Directory Structure / Cấu Trúc Thư Mục

* **Admin Directory (`admin`)**: The `admin` directory contains the backend files.
  * *Ghi chú (VN): Thư mục `admin` chứa các tệp tin mã nguồn của hệ thống quản trị (backend - Laravel).*
* **PC Frontend (`web`)**: The `web` directory is for the PC/Desktop client.
  * *Ghi chú (VN): Thư mục `web` dành cho giao diện máy tính (PC).*
* **Mobile Frontend (`m` or `wap`)**: The `m` or `wap` directory is for the mobile client.
  * *Ghi chú (VN): Thư mục `m` hoặc `wap` dành cho giao diện điện thoại di động.*

## 2. Configuration Steps / Các Bước Cấu Hình Thủ Công

1. **Backend Environment Configuration**:
   * **EN**: Open the `.env` file in the `admin` directory to modify the domain name and database connection information.
   * **VN**: Mở tệp `.env` trong thư mục `admin` để sửa đổi tên miền (tại dòng `APP_URL`) và thông tin kết nối cơ sở dữ liệu (`DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`).

2. **Frontend JS Configuration (File 1 & File 2)**:
   * **EN**: Open the `wap\static\configs.js` file (and the corresponding `configs.js` file in the `web` directory), search for `127.0` to locate the local IP address, and replace it with the `APP_URL` domain defined in the `admin/.env` file. **Note: It must be an official/live domain name, not a local address (localhost/127.0.0.1).**
   * **VN**: Mở tệp `wap\static\configs.js` (và tệp `configs.js` tương ứng trong thư mục `web` hoặc `m`), tìm kiếm từ khóa `127.0` để tìm địa chỉ cấu hình cũ và thay thế bằng tên miền `APP_URL` đã khai báo trong tệp `.env` của thư mục `admin`. **Lưu ý: Bắt buộc phải sử dụng tên miền chính thức, không được dùng địa chỉ nội bộ (localhost).**

3. **Domain Consistency**:
   * **EN**: The domain names modified in the two JS files must strictly match the `APP_URL` domain in the `admin/.env` file. **Note: It must be an official/live domain name, not a local address.**
   * **VN**: Tên miền được sửa đổi trong 2 tệp JS bắt buộc phải khớp hoàn toàn với tên miền `APP_URL` trong tệp `.env` của thư mục `admin`.

4. **Apply Changes**:
   * **EN**: After making the modifications, clear your browser cache and refresh the webpage.
   * **VN**: Sau khi sửa đổi xong, hãy xóa bộ nhớ đệm (cache) của trình duyệt và tải lại trang web.

## 3. Server Environment Requirements / Yêu Cầu Môi Trường Máy Chủ

* **Linux Baota (aaPanel) Environment**:
  * **EN**: If setting up with Linux Baota panel: use **Nginx or Apache + MySQL 5.7 + PHP 7.3**. PHP requires the `fileinfo` extension to be installed.
  * **VN**: Nếu cài đặt trên môi trường Linux Baota (aaPanel): sử dụng **Nginx hoặc Apache + MySQL 5.7 + PHP 7.3**. Trong phần quản lý PHP 7.3, bắt buộc phải cài đặt thêm tiện ích mở rộng (extension) `fileinfo`.
* **Windows Baota (aaPanel) Environment**:
  * **EN**: If setting up with Windows Baota panel: use **Nginx or Apache + MySQL 5.7 + PHP 7.3**. PHP requires the `fileinfo` extension to be installed.
  * **VN**: Nếu cài đặt trên môi trường Windows Baota: sử dụng **Nginx hoặc Apache + MySQL 5.7 + PHP 7.3**. PHP cũng cần phải cài đặt tiện ích mở rộng `fileinfo`.
* **Windows UPUPW ANK Environment**:
  * **EN**: If setting up with Windows UPUPW ANK environment: use **Nginx or Apache + Default MySQL + PHP 7.3**.
  * **VN**: Nếu cài đặt bằng phần mềm UPUPW ANK trên Windows: sử dụng **Nginx hoặc Apache + MySQL mặc định + PHP 7.3**.
* **Warning / Cảnh báo**:
  * **EN**: Xiaopi Panel (phpStudy) is **not recommended**.
  * **VN**: **Không khuyến khích** sử dụng phần mềm Tiểu Bì (Xiaopi / phpStudy) vì dễ phát sinh lỗi tương thích.

## 4. Data Collection / Thu Thập Dữ Liệu (Cronjob)

* **Game Record Collection URL**:
  * **EN**: The game record collection address is: `Backend_Domain/pull` (e.g., `https://www.h12368.com/pull`).
  * **VN**: Địa chỉ thu thập lịch sử trò chơi (dùng để chạy Cronjob/tác vụ tự động) là: `Tên_miền_admin/pull` (Ví dụ: `https://www.h12368.com/pull`).

## 5. URL Rewrite Rules / Cấu Hình Đường Dẫn Tĩnh (伪静态)

### 1. Apache URL Rewrite (Apache伪静态)

```apache
Options +FollowSymLinks -Indexes
RewriteEngine On
RewriteCond %{HTTP:Authorization} .
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [L]
```

### 2. Nginx URL Rewrite (Nginx 伪静态)

```nginx
location / {
     try_files $uri $uri/ /index.php?$query_string;
}
```

---

## 6. Smart Multi-Project Automated Deployment Script / Tập Lệnh Cài Đặt Tự Động Đa Dự Án (Tái Sử Dụng Nhiều Lần)

* **EN**: Below is a smart, reusable bash script suitable for `002bob` and any similar projects (`admin` + `web` + `wap/m`). It scans the system for existing installations, checks if `.env` and databases are already configured, prompts or automates **Overwrite vs. Keep DB vs. Create New Random DB**, generates random credentials/domains if left blank, and exports all credentials & configurations into `.keypass.txt`.
* **VN**: Tập lệnh dưới đây được thiết kế để **tái sử dụng nhiều lần cho hàng loạt dự án có cấu trúc tương tự (`admin` + `web` + `wap/m`)**. Các tính năng thông minh bao gồm:
  1. **Tự động quét hệ thống (System & Project Scanner)**: Kiểm tra Nginx, PHP 7.3 (`fileinfo`), MySQL, tự động dò mật khẩu Root MySQL trên Baota/aaPanel nếu bạn để trống.
  2. **Kiểm tra trạng thái dự án & Database**: Phát hiện xem dự án đã từng cài đặt chưa, tệp `.env` đã có thông tin kết nối chưa, Database đã có bảng dữ liệu chưa.
  3. **Tùy chọn linh hoạt khi chạy lại (Re-run Modes)**: Nếu phát hiện đã cài đặt, cho phép chọn:
     * `[1]` **Giữ nguyên DB cũ**, chỉ cập nhật lại Domain mới / cấu hình JS / Nginx.
     * `[2]` **Ghi đè (Reset)** lại trên chính DB cũ.
     * `[3]` **Tạo mới hoàn toàn** một DB ngẫu nhiên (Random) và thay thế vào `.env`.
  4. **Tự động Random biến thiếu**: Nếu không truyền Domain hoặc thông tin DB, script tự động sinh chuỗi ngẫu nhiên an toàn (`DB_NAME`, `DB_USER`, `DB_PASS`, `BASE_DOMAIN`).
  5. **Xuất hồ sơ bảo mật `.keypass.txt`**: Ghi lại toàn bộ lịch sử cài đặt, đường dẫn, tài khoản Database, danh sách Domain và lệnh Cronjob vào tệp `.keypass.txt` (phân quyền `600` chỉ root đọc được).

### Cách sử dụng (Usage):

**Cách 1: Chạy trực tiếp (Tự động tạo Random nếu chưa điền biến hoặc hỏi tương tác khi phát hiện bản cài cũ)**
```bash
nano deploy.sh
chmod +x deploy.sh
./deploy.sh
```

**Cách 2: Tái sử dụng nhanh cho nhiều dự án khác nhau bằng tham số dòng lệnh (CLI Arguments)**
```bash
# Cú pháp: ./deploy.sh <GIT_REPO_URL> <BASE_DOMAIN> <CHẾ_ĐỘ: auto|keep|overwrite|new>
./deploy.sh "https://github.com/limealisa/002bob-.git" "bob01.com" "auto"
./deploy.sh "https://github.com/limealisa/002bob-.git" "bob02.com" "new"
```

### Nội dung tập lệnh (`deploy.sh`):

```bash
#!/bin/bash
# ==============================================================================
# SMART REUSABLE ALL-IN-ONE DEPLOYMENT SCRIPT (FOR 002BOB & SIMILAR PROJECTS)
# Features:
#  - System & Environment Auto-Scanner (Baota/aaPanel & Standalone Linux)
#  - Existing Installation & Database State Detector
#  - Interactive or Automated Modes: Keep DB / Overwrite DB / New Random DB
#  - Auto-Random Generator for missing Variables (Domain, DB Name, User, Pass)
#  - Exports full deployment credentials to .keypass.txt
# ==============================================================================

set -e

# --- 1. NHẬN THAM SỐ HOẶC CẤU HÌNH MẶC ĐỊNH (PARAMETERS & DEFAULTS) ---
REPO_URL="${1:-https://github.com/limealisa/002bob-.git}"
INPUT_BASE_DOMAIN="${2:-}"       # Để trống sẽ tự động random domain hoặc lấy lại domain cũ
DEPLOY_MODE="${3:-ask}"          # ask | auto | keep | overwrite | new
PROTOCOL="${PROTOCOL:-http}"     # http hoặc https

# Tự động trích xuất tên dự án từ URL Git (VD: 002bob- -> 002bob)
PROJECT_SLUG=$(basename "$REPO_URL" .git | sed 's/[^a-zA-Z0-9_-]//g' | sed 's/-$//')
[ -z "$PROJECT_SLUG" ] && PROJECT_SLUG="game_project"

# Nếu truyền domain riêng, có thể tách thư mục theo domain để cài nhiều site trên 1 VPS
if [ -n "$INPUT_BASE_DOMAIN" ]; then
    FOLDER_NAME="${PROJECT_SLUG}_${INPUT_BASE_DOMAIN//./_}"
else
    FOLDER_NAME="${PROJECT_SLUG}"
fi

WWWROOT_DIR="/www/wwwroot"
[ ! -d "$WWWROOT_DIR" ] && WWWROOT_DIR="/var/www"
PROJECT_DIR="${PROJECT_DIR:-${WWWROOT_DIR}/${FOLDER_NAME}}"

# Mật khẩu root MySQL (Để trống script sẽ tự động quét từ cấu hình Baota/aaPanel hoặc không mật khẩu)
MYSQL_ROOT_PASS="${MYSQL_ROOT_PASS:-}"

# Hàm sinh chuỗi ngẫu nhiên an toàn
gen_random_str() {
    local length="${1:-8}"
    tr -dc 'a-z0-9' </dev/urandom | head -c "$length"
}

gen_random_pass() {
    local length="${1:-16}"
    tr -dc 'A-Za-z0-9_@#%' </dev/urandom | head -c "$length"
}

RAND_SUFFIX=$(gen_random_str 6)

echo "======================================================================"
echo " [SMART DEPLOYER] KHỞI ĐỘNG TRÌNH QUÉT & CÀI ĐẶT DỰ ÁN: ${PROJECT_SLUG}"
echo "======================================================================"

# --- 2. QUÉT HỆ THỐNG & KIỂM TRA MÔI TRƯỜNG (SYSTEM SCANNER) ---
echo "[1/8] Đang quét hệ thống và kiểm tra môi trường máy chủ..."

if [ -d "/www/server/panel" ]; then
    ENV_TYPE="BAOTA_AAPANEL"
    WEB_USER="www"
    WEB_GROUP="www"
    NGINX_VHOST_DIR="/www/server/panel/vhost/nginx"
    PHP_BIN="/www/server/php/73/bin/php"
    [ ! -x "$PHP_BIN" ] && PHP_BIN=$(command -v php || echo "")
    # Tự động lấy mật khẩu root MySQL từ Baota/aaPanel nếu người dùng chưa điền
    if [ -z "$MYSQL_ROOT_PASS" ] && [ -f "/www/server/panel/config/config.json" ]; then
        MYSQL_ROOT_PASS=$(grep -oP '"mysql_root"\s*:\s*"\K[^"]+' /www/server/panel/config/config.json 2>/dev/null || true)
    fi
else
    ENV_TYPE="STANDALONE_LINUX"
    WEB_USER="www-data"
    WEB_GROUP="www-data"
    NGINX_VHOST_DIR="/etc/nginx/conf.d"
    PHP_BIN=$(command -v php || echo "")
fi

echo " -> Môi trường phát hiện : ${ENV_TYPE}"
echo " -> Thư mục cài đặt đích : ${PROJECT_DIR}"

# Cài đặt công cụ cơ bản nếu thiếu
if ! command -v git &> /dev/null || ! command -v unzip &> /dev/null; then
    echo " -> Đang cài đặt bổ sung git, unzip, curl..."
    if [ -f /etc/debian_version ]; then
        apt-get update -y && apt-get install -y git unzip curl
    elif [ -f /etc/redhat-release ]; then
        yum install -y git unzip curl
    fi
fi

# Kiểm tra PHP fileinfo extension
if [ -n "$PHP_BIN" ] && [ -x "$PHP_BIN" ]; then
    if ! $PHP_BIN -m | grep -qi "fileinfo"; then
        echo " -> [CẢNH BÁO] PHP chưa bật tiện ích 'fileinfo'. Đang thử cài đặt tự động..."
        if [ "$ENV_TYPE" = "STANDALONE_LINUX" ] && [ -f /etc/debian_version ]; then
            apt-get install -y php7.3-fileinfo || true
        fi
    else
        echo " -> PHP fileinfo         : ĐÃ BẬT (OK)"
    fi
fi

# Kiểm tra kết nối MySQL Root
MYSQL_CMD="mysql -uroot"
if [ -n "$MYSQL_ROOT_PASS" ]; then
    MYSQL_CMD="mysql -uroot -p${MYSQL_ROOT_PASS}"
fi

if ! $MYSQL_CMD -e "SELECT 1;" &>/dev/null; then
    echo " -> Không thể tự động đăng nhập MySQL root. Vui lòng nhập mật khẩu root MySQL:"
    read -rsp "MySQL Root Password: " MYSQL_ROOT_PASS
    echo ""
    MYSQL_CMD="mysql -uroot -p${MYSQL_ROOT_PASS}"
    $MYSQL_CMD -e "SELECT 1;" &>/dev/null || { echo "Lỗi: Mật khẩu MySQL Root không chính xác!"; exit 1; }
fi
echo " -> Kết nối MySQL Root   : THÀNH CÔNG (OK)"

# --- 3. QUÉT TRẠNG THÁI CÀI ĐẶT CŨ (EXISTING INSTALLATION SCANNER) ---
echo "[2/8] Đang kiểm tra trạng thái cài đặt và cấu hình hiện có..."

IS_INSTALLED=0
IS_CONFIGURED=0
OLD_APP_URL=""
OLD_DB_NAME=""
OLD_DB_USER=""
OLD_DB_PASS=""
OLD_BASE_DOMAIN=""

if [ -d "$PROJECT_DIR" ] && [ -n "$(ls -A "$PROJECT_DIR" 2>/dev/null)" ]; then
    IS_INSTALLED=1
    if [ -f "$PROJECT_DIR/admin/.env" ]; then
        IS_CONFIGURED=1
        OLD_APP_URL=$(grep -E '^APP_URL=' "$PROJECT_DIR/admin/.env" | cut -d'=' -f2- | tr -d '"\r' || true)
        OLD_DB_NAME=$(grep -E '^DB_DATABASE=' "$PROJECT_DIR/admin/.env" | cut -d'=' -f2- | tr -d '"\r' || true)
        OLD_DB_USER=$(grep -E '^DB_USERNAME=' "$PROJECT_DIR/admin/.env" | cut -d'=' -f2- | tr -d '"\r' || true)
        OLD_DB_PASS=$(grep -E '^DB_PASSWORD=' "$PROJECT_DIR/admin/.env" | cut -d'=' -f2- | tr -d '"\r' || true)
        OLD_BASE_DOMAIN=$(echo "$OLD_APP_URL" | sed -E 's|https?://||' | sed -E 's|^admin\.||' | cut -d'/' -f1)
    fi
fi

# Kiểm tra xem Database cũ có tồn tại thực tế trong MySQL không
DB_EXISTS_IN_MYSQL=0
if [ -n "$OLD_DB_NAME" ]; then
    if $MYSQL_CMD -e "USE \`${OLD_DB_NAME}\`;" &>/dev/null; then
        DB_EXISTS_IN_MYSQL=1
    fi
fi

ACTION_DB="CREATE_NEW" # CREATE_NEW | OVERWRITE_OLD | KEEP_OLD

if [ "$IS_INSTALLED" -eq 1 ]; then
    echo "----------------------------------------------------------------------"
    echo " [PHÁT HIỆN DỰ ÁN ĐÃ TỒN TẠI TRÊN HỆ THỐNG]"
    echo "  - Thư mục hiện tại : $PROJECT_DIR"
    echo "  - Đã cấu hình .env : $([ "$IS_CONFIGURED" -eq 1 ] && echo "CÓ ($OLD_APP_URL)" || echo "CHƯA")"
    echo "  - Database cũ      : ${OLD_DB_NAME:-Chưa có} ($([ "$DB_EXISTS_IN_MYSQL" -eq 1 ] && echo "Đang tồn tại trên MySQL" || echo "Chưa tạo trên MySQL"))"
    echo "----------------------------------------------------------------------"

    if [ "$DEPLOY_MODE" = "ask" ]; then
        echo "Bạn muốn xử lý thế nào với lần chạy này?"
        echo "  [1] Giữ nguyên Database cũ (Chỉ cập nhật code / đổi Domain / sửa configs.js)"
        echo "  [2] Cài lại & Ghi đè (Xóa dữ liệu cũ, nạp lại SQL vào DB hiện tại)"
        echo "  [3] Tạo Database Random MỚI hoàn toàn và ghi đè cấu hình mới"
        read -rp "Chọn chế độ [1/2/3] (Mặc định: 1): " USER_CHOICE
        case "${USER_CHOICE:-1}" in
            1) ACTION_DB="KEEP_OLD" ;;
            2) ACTION_DB="OVERWRITE_OLD" ;;
            3) ACTION_DB="CREATE_NEW" ;;
            *) ACTION_DB="KEEP_OLD" ;;
        esac
    elif [ "$DEPLOY_MODE" = "keep" ] || [ "$DEPLOY_MODE" = "auto" ]; then
        [ "$DB_EXISTS_IN_MYSQL" -eq 1 ] && ACTION_DB="KEEP_OLD" || ACTION_DB="CREATE_NEW"
    elif [ "$DEPLOY_MODE" = "overwrite" ]; then
        [ "$DB_EXISTS_IN_MYSQL" -eq 1 ] && ACTION_DB="OVERWRITE_OLD" || ACTION_DB="CREATE_NEW"
    elif [ "$DEPLOY_MODE" = "new" ]; then
        ACTION_DB="CREATE_NEW"
    fi
else
    echo " -> Chưa phát hiện bản cài đặt cũ. Sẽ tiến hành cài mới hoàn toàn."
    ACTION_DB="CREATE_NEW"
fi

# --- 4. THIẾT LẬP BIẾN HOẶC TỰ ĐỘNG RANDOM (VARIABLES & RANDOMIZER) ---
echo "[3/8] Đang thiết lập thông số Tên miền và Database..."

# 4.1 Xử lý Tên miền (Nếu không nhập -> dùng lại domain cũ -> nếu chưa có thì random)
if [ -n "$INPUT_BASE_DOMAIN" ]; then
    BASE_DOMAIN="$INPUT_BASE_DOMAIN"
elif [ -n "$OLD_BASE_DOMAIN" ] && [ "$OLD_BASE_DOMAIN" != "127.0.0.1" ] && [ "$OLD_BASE_DOMAIN" != "localhost" ]; then
    BASE_DOMAIN="$OLD_BASE_DOMAIN"
else
    BASE_DOMAIN="${PROJECT_SLUG}-${RAND_SUFFIX}.com"
    echo " -> [AUTO-RANDOM] Chưa cung cấp tên miền, tự động tạo tên miền: ${BASE_DOMAIN}"
fi

ADMIN_DOMAIN="${ADMIN_DOMAIN:-admin.${BASE_DOMAIN}}"
WEB_DOMAIN="${WEB_DOMAIN:-www.${BASE_DOMAIN}}"
WAP_DOMAIN="${WAP_DOMAIN:-m.${BASE_DOMAIN}}"
APP_URL_FULL="${PROTOCOL}://${ADMIN_DOMAIN}"

# 4.2 Xử lý thông tin Database theo chế độ đã chọn
if [ "$ACTION_DB" = "KEEP_OLD" ] || [ "$ACTION_DB" = "OVERWRITE_OLD" ]; then
    if [ -n "$OLD_DB_NAME" ] && [ -n "$OLD_DB_USER" ] && [ -n "$OLD_DB_PASS" ]; then
        DB_NAME="$OLD_DB_NAME"
        DB_USER="$OLD_DB_USER"
        DB_PASS="$OLD_DB_PASS"
        echo " -> Sử dụng lại thông tin Database hiện có: ${DB_NAME}"
    else
        ACTION_DB="CREATE_NEW"
    fi
fi

if [ "$ACTION_DB" = "CREATE_NEW" ]; then
    SAFE_SLUG=$(echo "$PROJECT_SLUG" | tr -dc 'a-zA-Z0-9' | head -c 8)
    DB_NAME="${DB_NAME:-db_${SAFE_SLUG}_${RAND_SUFFIX}}"
    DB_USER="${DB_USER:-usr_${SAFE_SLUG}_${RAND_SUFFIX}}"
    DB_PASS="${DB_PASS:-$(gen_random_pass 16)}"
    echo " -> [AUTO-RANDOM] Đã khởi tạo thông tin Database mới: ${DB_NAME}"
fi

# --- 5. TẢI HOẶC CẬP NHẬT MÃ NGUỒN (CLONE / UPDATE REPOSITORY) ---
echo "[4/8] Đang đồng bộ mã nguồn từ $REPO_URL..."
mkdir -p $(dirname "$PROJECT_DIR")

if [ -d "$PROJECT_DIR/.git" ]; then
    echo " -> Kho Git đã tồn tại, đang đặt lại và kéo bản cập nhật mới..."
    cd "$PROJECT_DIR"
    git fetch --all
    git reset --hard origin/HEAD || git pull
else
    rm -rf "$PROJECT_DIR"
    git clone "$REPO_URL" "$PROJECT_DIR"
    cd "$PROJECT_DIR"
fi

# Tự động giải nén nếu trong kho chứa các tệp .zip
find "$PROJECT_DIR" -maxdepth 1 -name "*.zip" -exec unzip -o {} -d "$PROJECT_DIR" \;

# --- 6. KHỞI TẠO & NẠP CƠ SỞ DỮ LIỆU (DATABASE PROVISIONING) ---
echo "[5/8] Đang xử lý Cơ sở dữ liệu MySQL (Chế độ: ${ACTION_DB})..."

if [ "$ACTION_DB" = "OVERWRITE_OLD" ]; then
    echo " -> Đang xóa dữ liệu cũ và tạo lại Database ${DB_NAME}..."
    $MYSQL_CMD -e "DROP DATABASE IF EXISTS \`${DB_NAME}\`;"
fi

$MYSQL_CMD -e "CREATE DATABASE IF NOT EXISTS \`${DB_NAME}\` CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
$MYSQL_CMD -e "CREATE USER IF NOT EXISTS '${DB_USER}'@'localhost' IDENTIFIED BY '${DB_PASS}';"
$MYSQL_CMD -e "ALTER USER '${DB_USER}'@'localhost' IDENTIFIED BY '${DB_PASS}';" 2>/dev/null || \
$MYSQL_CMD -e "SET PASSWORD FOR '${DB_USER}'@'localhost' = PASSWORD('${DB_PASS}');" 2>/dev/null || true
$MYSQL_CMD -e "GRANT ALL PRIVILEGES ON \`${DB_NAME}\`.* TO '${DB_USER}'@'localhost'; FLUSH PRIVILEGES;"

if [ "$ACTION_DB" = "CREATE_NEW" ] || [ "$ACTION_DB" = "OVERWRITE_OLD" ]; then
    SQL_FILE=$(find "$PROJECT_DIR" -maxdepth 2 -name "*.sql" | head -n 1)
    if [ -n "$SQL_FILE" ]; then
        echo " -> Đang nạp tệp SQL ($SQL_FILE) vào Database ${DB_NAME}..."
        mysql -u"$DB_USER" -p"$DB_PASS" "$DB_NAME" < "$SQL_FILE"
        echo " -> Nạp SQL hoàn tất!"
    else
        echo " -> [LƯU Ý] Không tìm thấy tệp .sql trong thư mục gốc để nạp tự động."
    fi
else
    echo " -> Đã giữ nguyên toàn bộ dữ liệu trong Database ${DB_NAME}."
fi

# --- 7. CẬP NHẬT TỆP CẤU HÌNH .ENV & CONFIGS.JS (CONFIG REPLACEMENT) ---
echo "[6/8] Đang ghi đè biến Domain & Database vào admin/.env và các tệp configs.js..."

if [ ! -f "$PROJECT_DIR/admin/.env" ] && [ -f "$PROJECT_DIR/admin/.env.example" ]; then
    cp "$PROJECT_DIR/admin/.env.example" "$PROJECT_DIR/admin/.env"
fi

if [ -f "$PROJECT_DIR/admin/.env" ]; then
    sed -i "s|^APP_URL=.*|APP_URL=${APP_URL_FULL}|g" "$PROJECT_DIR/admin/.env"
    sed -i "s|^DB_DATABASE=.*|DB_DATABASE=${DB_NAME}|g" "$PROJECT_DIR/admin/.env"
    sed -i "s|^DB_USERNAME=.*|DB_USERNAME=${DB_USER}|g" "$PROJECT_DIR/admin/.env"
    sed -i "s|^DB_PASSWORD=.*|DB_PASSWORD=${DB_PASS}|g" "$PROJECT_DIR/admin/.env"
fi

# Thay thế cả địa chỉ 127.0.x.x lẫn tên miền cũ (nếu chạy lại đổi tên miền) trong toàn bộ configs.js
find "$PROJECT_DIR" -type f -name "configs.js" | while read -r js_file; do
    echo " -> Cập nhật Domain tại: $js_file"
    sed -i -E "s|https?://127\.0\.[0-9]+\.[0-9]+(:[0-9]+)?|${APP_URL_FULL}|g" "$js_file"
    sed -i -E "s|127\.0\.[0-9]+\.[0-9]+(:[0-9]+)?|${ADMIN_DOMAIN}|g" "$js_file"
    if [ -n "$OLD_APP_URL" ] && [ "$OLD_APP_URL" != "$APP_URL_FULL" ]; then
        sed -i "s|${OLD_APP_URL}|${APP_URL_FULL}|g" "$js_file"
    fi
done

# --- 8. CẤP QUYỀN, TRỎ DOMAIN NGINX & CRONJOB (PERMISSIONS & VHOST) ---
echo "[7/8] Đang phân quyền thư mục, tạo Nginx Vhost và cài đặt Cronjob..."

id -u $WEB_USER &>/dev/null || useradd -r -s /sbin/nologin $WEB_USER
chown -R ${WEB_USER}:${WEB_GROUP} "$PROJECT_DIR"
find "$PROJECT_DIR" -type d -exec chmod 755 {} \;
find "$PROJECT_DIR" -type f -exec chmod 644 {} \;
[ -d "$PROJECT_DIR/admin/storage" ] && chmod -R 775 "$PROJECT_DIR/admin/storage"
[ -d "$PROJECT_DIR/admin/bootstrap/cache" ] && chmod -R 775 "$PROJECT_DIR/admin/bootstrap/cache"

if [ -f "$PROJECT_DIR/admin/artisan" ] && [ -n "$PHP_BIN" ] && [ -x "$PHP_BIN" ]; then
    (cd "$PROJECT_DIR/admin" && $PHP_BIN artisan config:clear && $PHP_BIN artisan cache:clear) &>/dev/null || true
fi

mkdir -p "$NGINX_VHOST_DIR"
if [ "$ENV_TYPE" = "BAOTA_AAPANEL" ]; then
    PHP_FPM_CONF="include enable-php-73.conf;"
else
    PHP_FPM_CONF="location ~ \.php$ { fastcgi_pass unix:/run/php/php7.3-fpm.sock; fastcgi_index index.php; include fastcgi_params; fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name; }"
fi

ADMIN_ROOT="$PROJECT_DIR/admin/public"
[ ! -d "$ADMIN_ROOT" ] && ADMIN_ROOT="$PROJECT_DIR/admin"
WAP_ROOT="$PROJECT_DIR/wap"
[ ! -d "$WAP_ROOT" ] && [ -d "$PROJECT_DIR/m" ] && WAP_ROOT="$PROJECT_DIR/m"
WEB_ROOT="$PROJECT_DIR/web"

# Tạo Vhost cho Admin (kèm Nginx伪静态)
cat > "${NGINX_VHOST_DIR}/${ADMIN_DOMAIN}.conf" <<EOF
server {
    listen 80;
    server_name ${ADMIN_DOMAIN};
    index index.php index.html index.htm;
    root ${ADMIN_ROOT};

    location / {
        try_files \$uri \$uri/ /index.php?\$query_string;
    }

    ${PHP_FPM_CONF}

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
EOF

# Tạo Vhost cho PC (Web)
cat > "${NGINX_VHOST_DIR}/${WEB_DOMAIN}.conf" <<EOF
server {
    listen 80;
    server_name ${WEB_DOMAIN};
    index index.html index.htm index.php;
    root ${WEB_ROOT};

    location / {
        try_files \$uri \$uri/ /index.html;
    }
}
EOF

# Tạo Vhost cho Mobile (Wap/m)
cat > "${NGINX_VHOST_DIR}/${WAP_DOMAIN}.conf" <<EOF
server {
    listen 80;
    server_name ${WAP_DOMAIN};
    index index.html index.htm index.php;
    root ${WAP_ROOT};

    location / {
        try_files \$uri \$uri/ /index.html;
    }
}
EOF

if command -v nginx &> /dev/null; then
    nginx -t &>/dev/null && (systemctl reload nginx 2>/dev/null || /etc/init.d/nginx reload 2>/dev/null || true)
fi

# Cập nhật Cronjob thu thập dữ liệu (/pull)
CRON_CMD="* * * * * curl -sS ${APP_URL_FULL}/pull >/dev/null 2>&1"
(crontab -l 2>/dev/null | grep -v "${ADMIN_DOMAIN}/pull"; echo "$CRON_CMD") | crontab -

# --- 9. XUẤT TOÀN BỘ THÔNG TIN RA TỆP .keypass.txt (EXPORT CREDENTIALS) ---
echo "[8/8] Đang lưu trữ toàn bộ thông tin cấu hình và mật khẩu vào .keypass.txt..."

KEYPASS_PROJECT="${PROJECT_DIR}/.keypass.txt"
KEYPASS_ROOT="/root/.keypass.txt"
TIMESTAMP=$(date "+%Y-%m-%d %H:%M:%S")

KEYPASS_CONTENT="======================================================================
HỒ SƠ TRIỂN KHAI DỰ ÁN (DEPLOYMENT CREDENTIALS) - [${TIMESTAMP}]
======================================================================
[1. THÔNG TIN DỰ ÁN / PROJECT INFO]
- Git Repository     : ${REPO_URL}
- Thư mục cài đặt    : ${PROJECT_DIR}
- Chế độ thực thi    : ${ACTION_DB}
- Loại môi trường    : ${ENV_TYPE}

[2. DANH SÁCH TÊN MIỀN / DOMAINS]
- Base Domain        : ${BASE_DOMAIN}
- Backend (APP_URL)  : ${APP_URL_FULL}
- PC Frontend (Web)  : ${PROTOCOL}://${WEB_DOMAIN}
- Mobile (Wap / m)   : ${PROTOCOL}://${WAP_DOMAIN}
- Game Pull Cron URL : ${APP_URL_FULL}/pull

[3. THÔNG TIN CƠ SỞ DỮ LIỆU / DATABASE]
- DB Host            : 127.0.0.1 (localhost)
- DB Name            : ${DB_NAME}
- DB Username        : ${DB_USER}
- DB Password        : ${DB_PASS}
- MySQL Root Pass    : ${MYSQL_ROOT_PASS}

[4. TỆP CẤU HÌNH ĐÃ TỰ ĐỘNG CẬP NHẬT / MODIFIED FILES]
- Env File           : ${PROJECT_DIR}/admin/.env
- Nginx Admin Vhost  : ${NGINX_VHOST_DIR}/${ADMIN_DOMAIN}.conf
- Nginx Web Vhost    : ${NGINX_VHOST_DIR}/${WEB_DOMAIN}.conf
- Nginx Wap Vhost    : ${NGINX_VHOST_DIR}/${WAP_DOMAIN}.conf
======================================================================
"

# Ghi vào cả thư mục dự án và nối thêm lịch sử vào /root/.keypass.txt để dễ quản lý tập trung
echo "$KEYPASS_CONTENT" > "$KEYPASS_PROJECT"
echo "$KEYPASS_CONTENT" >> "$KEYPASS_ROOT"
chmod 600 "$KEYPASS_PROJECT" "$KEYPASS_ROOT"

echo "$KEYPASS_CONTENT"
echo " [HOÀN TẤT] Toàn bộ thông tin đã được xuất ra:"
echo "  1. ${KEYPASS_PROJECT}"
echo "  2. ${KEYPASS_ROOT} (Lưu lịch sử tổng hợp của mọi dự án trên máy chủ)"
echo "======================================================================"
```
