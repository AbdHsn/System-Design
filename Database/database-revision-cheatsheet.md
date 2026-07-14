# Database Revision Cheatsheet

> কুইজ সেশন থেকে তৈরি — ভুল হওয়া বা গ্যাপ থাকা পয়েন্টগুলোর উপর ফোকাস করে লেখা। যখনই ভুলে যাবেন, এখানে চোখ বুলিয়ে নিন।

---

## ১. Keys

### Primary Key vs Unique Constraint
| | Primary Key | Unique Constraint |
|---|---|---|
| NULL allow? | না | হ্যাঁ (এক বা একাধিক, DB-ভেদে ভিন্ন — নিচে দেখুন) |
| সংখ্যা | টেবিলে **মাত্র ১টা** | টেবিলে **যতগুলো খুশি** |
| FK reference | হতে পারে | হতে পারে (FK শুধু PK না, যেকোনো unique column-কেও reference করতে পারে) |

### Candidate Key vs Super Key
- **Super key** = যেকোনো কলাম সেট যা দিয়ে row uniquely identify করা যায় (বাড়তি কলাম থাকলেও চলবে)
- **Candidate key** = **minimal** super key (একটাও কলাম বাদ দিলে uniqueness থাকে না)
- সব candidate key super key, কিন্তু সব super key candidate key না
- Candidate key-গুলোর একটাকে বেছে নিলে সেটা **Primary Key**, বাকিগুলো **Alternate Key**

**Example (`students(student_id, email, phone, name)` — `student_id`/`email`/`phone` unique, `name` unique না):**

| Column সেট | Super Key? | Candidate Key? | কারণ |
|---|---|---|---|
| `{student_id}` | ✅ | ✅ | একাই unique, minimal |
| `{email}` | ✅ | ✅ | একাই unique, minimal |
| `{phone}` | ✅ | ✅ | একাই unique, minimal |
| `{student_id, email}` | ✅ | ❌ | unique ঠিকই, কিন্তু `email` বাদ দিলেও `student_id` একাই যথেষ্ট — বাড়তি |
| `{email, phone}` | ✅ | ❌ | দুটোই আলাদাভাবে unique, একসাথে রাখাই বাড়তি |
| `{name}` | ❌ | ❌ | `name` একা unique না |
| `{name, phone}` | ✅ | ❌ | unique হয় (phone-এর কারণে), কিন্তু `name` বাদ দিলেও চলে — minimal না |

**মনে রাখার সূত্র:** *বাড়তি কলাম বাদ দিলেও uniqueness থাকলে সেটা candidate key ছিল না (বাড়তি ছিল); বাদ দিলে uniqueness ভেঙে গেলে সেটাই candidate key।*
এখানে candidate key ৩টা (`student_id`, `email`, `phone`) — এর একটাকে PK বানালে বাকিগুলো Alternate Key।

### Composite Key
- একাধিক কলাম মিলে একটা key। যেমন: `order_items(order_id, product_id)` — junction/many-to-many টেবিলে কমন।
- **⚠️ মনে রাখার পয়েন্ট:** কলাম surrogate key (auto-increment) হলেও সেটা অন্য টেবিলে composite key-এর অংশ হতে কোনো বাধা নেই।

### Surrogate Key vs Natural Key
- **Natural key** = বাস্তব ডেটা থেকে আসা (email, ISBN, national ID) — বিজনেস মিনিং আছে
- **Surrogate key** = সিস্টেম-জেনারেটেড (auto-increment, UUID) — কোনো বিজনেস মিনিং নেই

**Surrogate key প্রেফার করার কারণ:**
- Natural key বদলাতে পারে (email change), কিন্তু PK কখনো বদলানো উচিত না
- Composite natural key হলে join জটিল ও ধীর
- Surrogate key ছোট/ফিক্সড সাইজ → index performance ভালো

**Natural key-এর জায়গা:** lookup/reference টেবিলে (যেমন `country_code = 'BD'`) readability-র জন্য মাঝে মাঝে ব্যবহার হয়।

---

## ২. Foreign Key — ON DELETE Policies

| Policy | আচরণ |
|---|---|
| `RESTRICT` / `NO ACTION` (**ডিফল্ট**) | Child row থাকা অবস্থায় parent ডিলিট **আটকে দেয়**, error দেয় — "violates foreign key constraint" |
| `CASCADE` | Parent ডিলিট হলে child row-ও **চুপচাপ ডিলিট** হয়ে যায় (কোনো error আসে না) |
| `SET NULL` | Parent ডিলিট হলে child-এর FK কলাম **NULL** হয়ে যায় — এজন্য FK কলাম অবশ্যই **nullable** হতে হবে |

**⚠️ ভুল ধারণা এড়ান:** "violates foreign key constraint" এরর মানে CASCADE দেওয়া আছে — বরং উল্টো, এটা আসে ডিফল্ট RESTRICT থাকলে।

**⚠️ SET NULL + NOT NULL কলাম হলে কী হয়:**
- FK কলাম `NOT NULL` হলেও `ON DELETE SET NULL` দিয়ে টেবিল/constraint **তৈরি করা যাবে** — DDL-এর সময় কোনো error আসে না, DB তখন চেক করে না।
- কিন্তু আসল **delete-এর সময়** যখন matching child row থাকবে, DB চেষ্টা করবে FK কলামে NULL বসাতে — আর সেটা `NOT NULL` constraint ভায়োলেট করে ফেলে।
- ফলাফল: **"null value violates not-null constraint"** (PostgreSQL) বা সমতুল্য **"Cannot insert the value NULL into column..."** (SQL Server) এরর → delete টাই ফেইল, parent row ডিলিট হয় না।

---

## ৩. DELETE vs TRUNCATE vs DROP

| | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| কী মুছে | নির্দিষ্ট row(s) | টেবিলের সব ডেটা + sequence reset | পুরো টেবিল স্ট্রাকচার + ডেটা |
| Logging | Row-by-row (ধীর), trigger fire করে | Minimally logged (দ্রুত), সাধারণত trigger fire করে না | — |
| Rollback | ✅ (PostgreSQL/SQL Server-এ DDL transactional) | ✅ (PostgreSQL/SQL Server) | ✅ (PostgreSQL/SQL Server) |

**⚠️ Cross-DB পার্থক্য:** MySQL-এ DDL transactional **না** — TRUNCATE/DROP সাথে সাথেই commit হয়ে যায়, rollback সম্ভব না। PostgreSQL ও SQL Server দুটোতেই rollback করা যায়।

---

## ৪. Normalization

### 1NF, 2NF, 3NF — সঠিক সংজ্ঞা
- **1NF:** প্রতিটা কলামের ভ্যালু **atomic** হতে হবে (array/comma-separated value চলবে না), প্রতিটা row unique হতে হবে।
- **2NF:** 1NF + **partial dependency নেই**। শুধু **composite PK** থাকলেই relevant — non-key কলাম পুরো composite key-এর উপর depend করতে হবে, শুধু অংশের উপর না।
  - উদাহরণ ভায়োলেশন: `order_items(order_id, product_id, product_name)` — `product_name` শুধু `product_id`-এর উপর নির্ভর করে।
- **3NF:** 2NF + **transitive dependency নেই** — non-key কলাম আরেকটা non-key কলামের উপর depend করতে পারবে না।
  - উদাহরণ ভায়োলেশন: `employee(emp_id, dept_id, dept_name)` — `dept_name` নির্ভর করে `dept_id`-এর উপর (যেটা নিজেই non-key)।

**⚠️ ভুল ধারণা এড়ান:** normalization মানে "redundant data সরিয়ে ডিটেইল টেবিল বানানো" — এটা ফলাফল, কিন্তু প্রতিটা NF-এর নির্দিষ্ট rule (atomic value / partial dependency / transitive dependency) মনে রাখা দরকার।

### BCNF (Boyce-Codd Normal Form)
- **Rule:** প্রতিটা functional dependency `X → Y`-তে `X` কে অবশ্যই **super key** হতে হবে।
- 3NF pass করেও redundancy থাকতে পারে যখন determinant (`X`) একটা **candidate key-এর অংশ** (super key না, partial key)।
- **ক্লাসিক উদাহরণ:** `course_instructor(student_id, course_id, instructor)` — rule: প্রতি course-এর একজনই instructor।
  - Candidate key: `(student_id, course_id)`
  - কিন্তু `course_id → instructor` — `course_id` নিজে super key না, তাই BCNF ভায়োলেশন
  - সমাধান: ভেঙে ফেলা — `course(course_id, instructor)` + `enrollment(student_id, course_id)`
- **সংক্ষেপে:** 3NF-এ "partial superkey determinant" এলাউড, BCNF-এ সেটাও নিষিদ্ধ।

### Denormalization
- ইচ্ছাকৃতভাবে redundancy রাখা হয় যখন **read/query speed** priority।
- উদাহরণ: e-commerce order history dashboard-এ বারবার `orders` + `order_items` + `products` join করা ধীর → তাই `order_summary` নামে denormalized টেবিল রাখা হয় (product name, price সব একসাথে)।
- ট্রেড-অফ: redundant ডেটা sync রাখতে হয় (trigger/batch job দিয়ে), কিন্তু read speed বাড়ে। Read-heavy analytics/reporting-এ কমন প্র্যাকটিস।

---

## ৫. NULL — কমন গোঁচকা (Gotchas)

### NULL + UNIQUE Constraint (Cross-DB পার্থক্য)
- **Standard SQL rule:** `NULL ≠ NULL` — তাই দুইটা NULL "একই ভ্যালু" ধরা হয় না।
- **PostgreSQL:** একাধিক NULL এলাউ করে unique কলামে, কোনো violation হয় না।
- **⚠️ SQL Server:** unique constraint ভেতরে unique **index** দিয়ে implement হয়, যা NULL-কেও ভ্যালু হিসেবে treat করে duplicate-check করে → **শুধু একটা NULL** ঢোকানো যায়, দ্বিতীয়টায় violation error!

### CHECK Constraint + NULL
- SQL-এ কোনো এক্সপ্রেশনে NULL থাকলে ফলাফল হয় **UNKNOWN** (TRUE/FALSE না)।
- CHECK constraint **শুধু তখনই ফেল করে** যখন এক্সপ্রেশন স্পষ্টভাবে **FALSE**। UNKNOWN হলে constraint **pass** ধরা হয়।
- **উদাহরণ:** `age INT CHECK (age >= 18)` — `NULL >= 18` ইভালুয়েট হয় UNKNOWN → CHECK এটাকে block করে না → **NULL ইনসার্ট করা যায়!**
- NULL আটকাতে হলে আলাদাভাবে `NOT NULL` যোগ করতে হবে — CHECK একা যথেষ্ট না।

---

## দ্রুত রিভিশন চেকলিস্ট
- [ ] PK vs Unique — সংখ্যা ও NULL rule
- [ ] Candidate key vs Super key — minimality
- [ ] FK ON DELETE — RESTRICT (default) vs CASCADE vs SET NULL
- [ ] DELETE/TRUNCATE/DROP — rollback (Postgres/SQL Server হ্যাঁ, MySQL না)
- [ ] 1NF/2NF/3NF — সঠিক rule (atomic / partial dep / transitive dep)
- [ ] BCNF — super key determinant rule
- [ ] NULL + UNIQUE — Postgres vs SQL Server পার্থক্য
- [ ] CHECK + NULL — UNKNOWN pass করে যায়

---
*সেশন তারিখ: ১৩ জুলাই, ২০২৬ — পরবর্তী রাউন্ডগুলো (SQL Joins, Transactions, Indexing) কভার হলে এই ফাইলে আপডেট হবে।*
