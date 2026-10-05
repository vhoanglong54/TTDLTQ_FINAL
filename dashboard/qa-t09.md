# T09 — Baseline đối chiếu Power BI

Các giá trị dưới đây được tính trực tiếp từ `data/processed/clean_dataset.csv` ở trạng thái **không có filter**. Power BI phải khớp trước khi tiếp tục T12.

## Baseline toàn bộ dữ liệu

| Measure | Expected |
|---|---:|
| Learning Attempts | 32.593 |
| Distinct Learners | 28.785 |
| At-Risk Count | 17.208 |
| At-Risk Rate | 52,7966% |
| Pass Count | 15.385 |
| Pass Rate | 47,2034% |
| Assessment Scored Count | 173.739 |
| Assessment Score Sum | 13.169.342 |
| Average Assessment Score | 75,7996 |
| VLE Total Clicks | 38.343.063 |
| Average VLE Clicks per Attempt | 1.176,4202 |

File input có SHA-256 `4b3250f59456d9eb546291c52bd7e3ff5b4afe0a942b54df961cd193a8137701`, 32.593 dòng và 35 cột.

## Baseline map theo region

| Region | Attempts | At-Risk Count | At-Risk Rate |
|---|---:|---:|---:|
| East Anglian Region | 3.340 | 1.704 | 51,0180% |
| East Midlands Region | 2.365 | 1.284 | 54,2918% |
| Ireland | 1.184 | 534 | 45,1014% |
| London Region | 3.216 | 1.854 | 57,6493% |
| North Region | 1.823 | 902 | 49,4789% |
| North Western Region | 2.906 | 1.738 | 59,8073% |
| Scotland | 3.446 | 1.759 | 51,0447% |
| South East Region | 2.111 | 1.024 | 48,5078% |
| South Region | 3.092 | 1.472 | 47,6067% |
| South West Region | 2.436 | 1.223 | 50,2053% |
| Wales | 2.086 | 1.144 | 54,8418% |
| West Midlands Region | 2.582 | 1.464 | 56,7002% |
| Yorkshire Region | 2.006 | 1.106 | 55,1346% |

## Checklist T09 trong Power BI Desktop

- [ ] Import đúng một bảng `clean_dataset`, 32.593 dòng và 35 cột.
- [ ] Tất cả core measure trong `measures.dax` được tạo và khớp baseline không filter.
- [ ] Slicer module/presentation/region thay đổi cả numerator và denominator đúng filter context.
- [ ] Azure Maps nhận diện/hiển thị đủ 13 region; chụp ảnh và ghi trường hợp geocode sai.
- [ ] Tooltip map hiển thị Attempts, At-Risk Count và At-Risk Rate.
- [ ] Page 1–3 có skeleton theo `wireframe.md`; Page 4 ghi rõ “Awaiting T13 model output”.
- [ ] Lưu `.pbix` và ảnh bằng chứng theo hướng dẫn trong `evidence/README.md`.

