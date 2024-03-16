<!--[meta]
section: api
subSection: utilities
title: App version plugin
[meta]-->

# Trình cắm cho phiên bản ứng dụng

Trả về phiên bản hiện tại của ứng dụng.

> Lưu ý sau khi phiên bản KeystoneJS 5 chuyển sang chế độ duy trì để ra mắt
> phiên bản mới hơn. Chúng tôi đã dựa trên mã nguồn cũ này để phát triển một
> phiên bản khác với một số tính năng theo hướng microservices.

Gói này cung cấp hỗ trợ để bao gồm chuỗi phiên bản dưới dạng HTTP tiêu đề phản
hồi và dưới dạng truy vấn graphQL.

Hàm `appVersionMiddleware(version)` sẽ trả về một phần middleware sẽ đặt tiêu đề
phản hồi `X-Ocop-App-Version` thành `version` trên tất cả HTTP yêu cầu.

Nhà cung cấp graphQL `AppVersionProvider` sẽ thêm truy vấn `{ appVersion }` vào
API graphQL của bạn trả về `version` dưới dạng một chuỗi.

## Hướng dẫn

###  Dùng gián tiếp

Gói này được thiết kế để sử dụng gián tiếp thông qua API tiện ích trên lớp
`Ocop`:

```javascript
const ocop = new Ocop({
  appVersion: {
    version: "1.0.0",
    addVersionToHttpHeaders: true,
    access: true,
  },
});
```

### Dùng trực tiếp

Nó cũng có thể được sử dụng trực tiếp nếu bạn muốn quản lý thủ công ngăn xếp
phần mềm trung gian của các nhà cung cấp GraphQL.

```javascript
const {
  AppVersionProvider,
  appVersionMiddleware,
} = require("@ocopjs/app-version");

const version = "1.0.0";

app.use(appVersionMiddleware(version));

ocop._providers.push(
  new AppVersionProvider({
    version,
    access: true,
    schemaNames: ["public"],
  }),
);
```
