# HNUE Map – bản đồ vector Đại học Sư phạm Hà Nội

Bản đồ khuôn viên HNUE và khu lân cận, vẽ bằng SVG theo phong cách các ảnh trong `assets/images`:
nền đào/be, đường xanh lam nhạt có viền, tòa nhà kem có bóng đổ, tán cây theo cụm, ao và đài phun nước xanh,
biểu tượng POI tròn (ăn uống cam, cửa hàng xanh, trường học nâu, khoa tím, bãi đỗ xe tím) và nhãn có viền sáng.

## Chạy
Giải nén, mở `index.html`. Không cần server hay bước build.

- `index.html` – bản đồ cho người dùng
- `editor.html` – công cụ vẽ / kéo / sửa bản đồ trực tiếp, xuất file vá `js/overrides.js`

## Tính năng
- Kéo để di chuyển, cuộn hoặc chụm hai ngón để zoom (vector, luôn nét). Nút +, −, ➤ ở góc trái dưới.
- Nhãn hiện dần theo mức zoom: tên khu vực → POI → mã tòa (B2, C4, D1…) → tên khoa.
- Ô Search góc phải trên (Ctrl+K), tìm không cần gõ dấu; mũi tên + Enter để chọn.
- Nút ☰ mở danh sách địa điểm, chú giải, bộ lọc; chip "Giảng đường / Ký túc xá / Dịch vụ" tô màu các tòa phù hợp.
- Bấm tòa nhà hoặc biểu tượng để xem thẻ chi tiết, bay tới vị trí, mở chỉ đường Google Maps.
- `index.html#D1` mở thẳng một địa điểm.

## Cấu trúc
```
index.html, editor.html
css/style.css          giao diện chung
css/editor.css         giao diện riêng của editor
js/data.js             THÔNG TIN CHỮ: tên, mô tả, loại, vùng + chú giải
js/overrides.js        file vá do editor sinh ra (ban đầu rỗng)
js/geo/                HÌNH HỌC bản đồ, mỗi file một lớp
  frames.js            hệ toạ độ tham chiếu (i1…i7, s1, s2, s5) quy ra world
  data-areas.js        đất, cỏ, nước, sân vận động, đài phun nước
  data-roads.js        đường xe, lối đi bộ, bãi đỗ xe
  data-buildings.js    tòa nhà, sân trong, khối nhà rải, nhà phố
  data-nature.js       vùng rải cây, hàng cây
  data-poi.js          nhãn / POI, cổng, điểm neo
  data-ext.js          BỔ SUNG theo ảnh chụp trong assets/images (xem bên dưới)
  build.js             biên dịch data → GEO + áp overrides.js
js/render.js           dựng SVG nền (đường, bóng, cây) từ GEO
js/map.js              zoom/kéo, nhãn theo zoom, tìm kiếm, lọc, thẻ chi tiết
js/editor.js           logic của editor.html
```
Nguyên tắc: sửa dữ liệu → `data-*.js` / `data.js`; sửa cách vẽ → `render.js`; sửa tương tác → `map.js`.

## data-ext.js đã bổ sung gì
Hệ toạ độ `s1`, `s2`, `s5` là ảnh chụp 185157, 185228, 185359 (khung gần đúng, bỏ qua độ nghiêng của ảnh).
- Khu phố nhà ống phía Bắc, Đông Ngõ 199 Trần Quốc Hoàn (Ngách 199/1, 199/3) và phía Tây (bốn dãy × năm cột, ngõ ngang dọc).
- Dải nhà xếp dọc phía Tây Phan Văn Trường (`v:true` trong `townhouses`).
- Phạm Văn Đồng / Vành đai 3 nhiều làn (lớp `hwy`, dải phân cách `median`).
- Khu KTX: A1–A5 đặt lại theo ảnh, các tòa bên cạnh, ngõ nhỏ, đất nền be.
- Khối phía Đông Phan Văn Trường: Học viện Tư pháp cũ, Binh chủng Hóa học, Bảo tàng…
- Tên đường (Ngõ 199, Ngách 199/1, 199/3, Ngõ 130 Xuân Thủy, Nguyễn Phong Sắc, Phạm Văn Đồng) và vài POI mới.
- Gỡ các mục ước lượng cũ bị thay thế (một số khối `fillBlocks`, vài đường lane, A1–A4 cũ). Việc này làm vị trí cây tự rải trong vùng đó đổi so với bản trước.

## Chỉnh bằng editor
Mở `editor.html`, kéo/zoom như bản đồ thường.

| Công cụ | Phím | Chức năng |
|---|---|---|
| Di chuyển | V | chọn và kéo tòa / nhãn / POI / đường |
| Đỉnh | E | kéo từng đỉnh đa giác |
| Nhà ▭ | R | kéo chuột tạo tòa chữ nhật |
| Nhà ⬠ | P | click từng điểm, Enter chốt |
| POI | A | click tạo điểm mới, sửa ở bảng phải |
| Đường | D | click từng điểm, Enter chốt |
| Vùng cây | T | kéo chuột tạo vùng rải cây |

Esc huỷ · Ctrl+Z hoàn tác · C nhân bản · Del xoá. Có autosave (localStorage), ⟲ Reset về dữ liệu gốc.

Lưu: **⭱ Xuất overrides.js** → Copy hoặc Tải file → thay `js/overrides.js` → mở lại `index.html`.
Muốn gộp vĩnh viễn: override có `key.id` → sửa mục có id đó trong data; có `key.at:[x,y]` → tìm mục gốc trùng toạ độ, sửa rồi xoá dòng override.

## Chỉnh bằng tay
- **Tòa nhà**: `js/geo/data-buildings.js`, mảng `items`, ví dụ `{ id:'X1', rect:[640,240,690,280], label:'X1', kind:'code' }` (toạ độ world; thêm `f:'i4'` để dùng toạ độ pixel ảnh tương ứng). `kind`: `code` (chữ mã) · `dot` (chấm + tên) · `bldg`.
- **POI**: thêm một dòng vào `labels` trong `data-poi.js`, kèm mục cùng `id` trong `surroundings` của `data.js` để bấm được.
- **Cây**: `data-nature.js`, vùng `{ rect:[…], n:20, r:[2.4,4] }` hoặc hàng `{ pts:[[x1,y1],[x2,y2]], step:8 }`. Cây tự né đường, tòa và nước.
- **Nhà phố**: mục `townhouses` `{ rect, w, d, lane }`; `single:true` mỗi dãy một hàng, `v:true` xếp dọc.

### Lưu ý
- Vị trí cây rải tự động phụ thuộc thứ tự mảng `fillBlocks` và `zones`; thêm mục mới ở cuối danh sách là an toàn.
- Vị trí một số tòa phía Bắc và POI quanh trường là ước lượng.
- Camera bị giới hạn bởi `bounds` trong `build.js` và vùng kẹp trong `map.js`; mở rộng bản đồ thì nới các giá trị này.
- Console cảnh báo `[overrides] không khớp` khi một dòng override không còn tìm thấy mục gốc → gộp vào `data-*.js` rồi xoá dòng đó.

