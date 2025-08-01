<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "40b1bbffdb8ce6812bf6e701cad876b6",
  "translation_date": "2025-07-17T18:08:30+00:00",
  "source_file": "03-GettingStarted/06-http-streaming/README.md",
  "language_code": "bn"
}
-->
# HTTPS স্ট্রিমিং মডেল কনটেক্সট প্রোটোকল (MCP) এর সাথে

এই অধ্যায়টি HTTPS ব্যবহার করে মডেল কনটেক্সট প্রোটোকল (MCP) এর মাধ্যমে নিরাপদ, স্কেলেবল এবং রিয়েল-টাইম স্ট্রিমিং বাস্তবায়নের ব্যাপক নির্দেশিকা প্রদান করে। এতে স্ট্রিমিংয়ের প্রেরণা, উপলব্ধ ট্রান্সপোর্ট মেকানিজম, MCP-তে স্ট্রিমেবল HTTP কিভাবে বাস্তবায়ন করবেন, নিরাপত্তার সেরা অনুশীলন, SSE থেকে মাইগ্রেশন এবং নিজের স্ট্রিমিং MCP অ্যাপ্লিকেশন তৈরির জন্য ব্যবহারিক নির্দেশনা অন্তর্ভুক্ত রয়েছে।

## MCP-তে ট্রান্সপোর্ট মেকানিজম এবং স্ট্রিমিং

এই অংশে MCP-তে উপলব্ধ বিভিন্ন ট্রান্সপোর্ট মেকানিজম এবং ক্লায়েন্ট ও সার্ভারের মধ্যে রিয়েল-টাইম যোগাযোগের জন্য স্ট্রিমিং সক্ষম করার তাদের ভূমিকা আলোচনা করা হয়েছে।

### ট্রান্সপোর্ট মেকানিজম কী?

ট্রান্সপোর্ট মেকানিজম নির্ধারণ করে কিভাবে ডেটা ক্লায়েন্ট এবং সার্ভারের মধ্যে বিনিময় হয়। MCP বিভিন্ন পরিবেশ এবং প্রয়োজন অনুযায়ী একাধিক ট্রান্সপোর্ট টাইপ সমর্থন করে:

- **stdio**: স্ট্যান্ডার্ড ইনপুট/আউটপুট, স্থানীয় এবং CLI-ভিত্তিক টুলের জন্য উপযুক্ত। সহজ কিন্তু ওয়েব বা ক্লাউডের জন্য উপযুক্ত নয়।
- **SSE (Server-Sent Events)**: সার্ভারকে HTTP এর মাধ্যমে ক্লায়েন্টকে রিয়েল-টাইম আপডেট পাঠানোর সুযোগ দেয়। ওয়েব UI এর জন্য ভালো, তবে স্কেলেবিলিটি এবং নমনীয়তায় সীমাবদ্ধ।
- **Streamable HTTP**: আধুনিক HTTP-ভিত্তিক স্ট্রিমিং ট্রান্সপোর্ট, যা নোটিফিকেশন এবং উন্নত স্কেলেবিলিটি সমর্থন করে। অধিকাংশ প্রোডাকশন এবং ক্লাউড পরিস্থিতির জন্য সুপারিশকৃত।

### তুলনামূলক টেবিল

নিচের তুলনামূলক টেবিলটি দেখে এই ট্রান্সপোর্ট মেকানিজমগুলোর পার্থক্য বুঝুন:

| ট্রান্সপোর্ট       | রিয়েল-টাইম আপডেট | স্ট্রিমিং | স্কেলেবিলিটি | ব্যবহারের ক্ষেত্র          |
|-------------------|------------------|-----------|-------------|-------------------------|
| stdio             | না               | না        | কম          | স্থানীয় CLI টুল         |
| SSE               | হ্যাঁ             | হ্যাঁ      | মাঝারি       | ওয়েব, রিয়েল-টাইম আপডেট |
| Streamable HTTP   | হ্যাঁ             | হ্যাঁ      | উচ্চ         | ক্লাউড, মাল্টি-ক্লায়েন্ট |

> **টিপ:** সঠিক ট্রান্সপোর্ট নির্বাচন পারফরম্যান্স, স্কেলেবিলিটি এবং ব্যবহারকারীর অভিজ্ঞতায় প্রভাব ফেলে। আধুনিক, স্কেলেবল এবং ক্লাউড-রেডি অ্যাপ্লিকেশনের জন্য **Streamable HTTP** সুপারিশ করা হয়।

আগের অধ্যায়গুলোতে stdio এবং SSE ট্রান্সপোর্ট দেখানো হয়েছে এবং এই অধ্যায়ে স্ট্রিমেবল HTTP ট্রান্সপোর্ট আলোচনা করা হয়েছে।

## স্ট্রিমিং: ধারণা এবং প্রেরণা

স্ট্রিমিংয়ের মৌলিক ধারণা এবং প্রেরণা বোঝা কার্যকর রিয়েল-টাইম যোগাযোগ ব্যবস্থা বাস্তবায়নের জন্য অপরিহার্য।

**স্ট্রিমিং** হল নেটওয়ার্ক প্রোগ্রামিংয়ের একটি কৌশল যা ডেটা ছোট, পরিচালনাযোগ্য অংশ বা ইভেন্টের ধারাবাহিকতা হিসেবে পাঠানো এবং গ্রহণ করার সুযোগ দেয়, পুরো রেসপন্স প্রস্তুত হওয়ার জন্য অপেক্ষা না করে। এটি বিশেষভাবে উপকারী:

- বড় ফাইল বা ডেটাসেটের জন্য।
- রিয়েল-টাইম আপডেট (যেমন, চ্যাট, প্রগ্রেস বার)।
- দীর্ঘমেয়াদী গণনার জন্য যেখানে ব্যবহারকারীকে অবহিত রাখতে হয়।

উচ্চ স্তরে স্ট্রিমিং সম্পর্কে যা জানা দরকার:

- ডেটা ধাপে ধাপে সরবরাহ করা হয়, একবারে নয়।
- ক্লায়েন্ট ডেটা আসার সাথে সাথে প্রক্রিয়া করতে পারে।
- অনুভূত লেটেন্সি কমায় এবং ব্যবহারকারীর অভিজ্ঞতা উন্নত করে।

### কেন স্ট্রিমিং ব্যবহার করবেন?

স্ট্রিমিং ব্যবহারের কারণগুলো হলো:

- ব্যবহারকারীরা অবিলম্বে প্রতিক্রিয়া পায়, শুধু শেষে নয়।
- রিয়েল-টাইম অ্যাপ্লিকেশন এবং প্রতিক্রিয়াশীল UI সক্ষম করে।
- নেটওয়ার্ক এবং কম্পিউট রিসোর্সের আরও দক্ষ ব্যবহার।

### সহজ উদাহরণ: HTTP স্ট্রিমিং সার্ভার ও ক্লায়েন্ট

নিচে স্ট্রিমিং কিভাবে বাস্তবায়ন করা যায় তার একটি সহজ উদাহরণ দেওয়া হলো:

## Python

**সার্ভার (Python, FastAPI এবং StreamingResponse ব্যবহার করে):**

### Python

```python
from fastapi import FastAPI
from fastapi.responses import StreamingResponse
import time

app = FastAPI()

async def event_stream():
    for i in range(1, 6):
        yield f"data: Message {i}\n\n"
        time.sleep(1)

@app.get("/stream")
def stream():
    return StreamingResponse(event_stream(), media_type="text/event-stream")
```

**ক্লায়েন্ট (Python, requests ব্যবহার করে):**

### Python

```python
import requests

with requests.get("http://localhost:8000/stream", stream=True) as r:
    for line in r.iter_lines():
        if line:
            print(line.decode())
```

এই উদাহরণে সার্ভার ক্লায়েন্টকে বার্তা সিরিজ পাঠায় যেমন তারা প্রস্তুত হয়, সব বার্তা একসাথে অপেক্ষা না করে।

**কিভাবে কাজ করে:**
- সার্ভার প্রতিটি বার্তা প্রস্তুত হওয়ার সাথে সাথে পাঠায়।
- ক্লায়েন্ট প্রতিটি অংশ পাওয়ার সাথে সাথে গ্রহণ ও প্রিন্ট করে।

**প্রয়োজনীয়তা:**
- সার্ভারকে স্ট্রিমিং রেসপন্স ব্যবহার করতে হবে (যেমন FastAPI তে `StreamingResponse`)।
- ক্লায়েন্টকে স্ট্রিম হিসেবে রেসপন্স প্রক্রিয়া করতে হবে (`stream=True` requests এ)।
- Content-Type সাধারণত `text/event-stream` বা `application/octet-stream` হয়।

## Java

**সার্ভার (Java, Spring Boot এবং Server-Sent Events ব্যবহার করে):**

```java
@RestController
public class CalculatorController {

    @GetMapping(value = "/calculate", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<ServerSentEvent<String>> calculate(@RequestParam double a,
                                                   @RequestParam double b,
                                                   @RequestParam String op) {
        
        double result;
        switch (op) {
            case "add": result = a + b; break;
            case "sub": result = a - b; break;
            case "mul": result = a * b; break;
            case "div": result = b != 0 ? a / b : Double.NaN; break;
            default: result = Double.NaN;
        }

        return Flux.<ServerSentEvent<String>>just(
                    ServerSentEvent.<String>builder()
                        .event("info")
                        .data("Calculating: " + a + " " + op + " " + b)
                        .build(),
                    ServerSentEvent.<String>builder()
                        .event("result")
                        .data(String.valueOf(result))
                        .build()
                )
                .delayElements(Duration.ofSeconds(1));
    }
}
```

**ক্লায়েন্ট (Java, Spring WebFlux WebClient ব্যবহার করে):**

```java
@SpringBootApplication
public class CalculatorClientApplication implements CommandLineRunner {

    private final WebClient client = WebClient.builder()
            .baseUrl("http://localhost:8080")
            .build();

    @Override
    public void run(String... args) {
        client.get()
                .uri(uriBuilder -> uriBuilder
                        .path("/calculate")
                        .queryParam("a", 7)
                        .queryParam("b", 5)
                        .queryParam("op", "mul")
                        .build())
                .accept(MediaType.TEXT_EVENT_STREAM)
                .retrieve()
                .bodyToFlux(String.class)
                .doOnNext(System.out::println)
                .blockLast();
    }
}
```

**Java বাস্তবায়নের নোট:**
- Spring Boot এর রিয়েক্টিভ স্ট্যাক `Flux` ব্যবহার করে স্ট্রিমিং করে
- `ServerSentEvent` ইভেন্ট টাইপসহ কাঠামোবদ্ধ ইভেন্ট স্ট্রিমিং প্রদান করে
- `WebClient` এর `bodyToFlux()` রিয়েক্টিভ স্ট্রিমিং গ্রহণ সক্ষম করে
- `delayElements()` ইভেন্টগুলোর মধ্যে প্রক্রিয়াকরণের সময় সিমুলেট করে
- ইভেন্টগুলোর টাইপ থাকতে পারে (`info`, `result`) যা ক্লায়েন্ট হ্যান্ডলিং উন্নত করে

### তুলনা: ক্লাসিক স্ট্রিমিং বনাম MCP স্ট্রিমিং

ক্লাসিক HTTP স্ট্রিমিং এবং MCP স্ট্রিমিংয়ের মধ্যে পার্থক্য নিচের টেবিলে দেখানো হলো:

| বৈশিষ্ট্য               | ক্লাসিক HTTP স্ট্রিমিং          | MCP স্ট্রিমিং (নোটিফিকেশন)       |
|------------------------|-------------------------------|----------------------------------|
| প্রধান রেসপন্স          | চাঙ্কড                        | একক, শেষে                       |
| প্রগ্রেস আপডেট         | ডেটা চাঙ্ক হিসেবে পাঠানো হয়  | নোটিফিকেশন হিসেবে পাঠানো হয়    |
| ক্লায়েন্টের প্রয়োজন   | স্ট্রিম প্রক্রিয়া করতে হবে    | মেসেজ হ্যান্ডলার বাস্তবায়ন করতে হবে |
| ব্যবহারের ক্ষেত্র       | বড় ফাইল, AI টোকেন স্ট্রিম    | প্রগ্রেস, লগ, রিয়েল-টাইম ফিডব্যাক |

### মূল পার্থক্যসমূহ

অতিরিক্ত কিছু মূল পার্থক্য:

- **যোগাযোগ প্যাটার্ন:**
   - ক্লাসিক HTTP স্ট্রিমিং: সহজ চাঙ্কড ট্রান্সফার এনকোডিং ব্যবহার করে ডেটা পাঠায়
   - MCP স্ট্রিমিং: JSON-RPC প্রোটোকল সহ কাঠামোবদ্ধ নোটিফিকেশন সিস্টেম ব্যবহার করে

- **মেসেজ ফরম্যাট:**
   - ক্লাসিক HTTP: নতুন লাইনের সাথে প্লেইন টেক্সট চাঙ্ক
   - MCP: মেটাডেটাসহ কাঠামোবদ্ধ LoggingMessageNotification অবজেক্ট

- **ক্লায়েন্ট বাস্তবায়ন:**
   - ক্লাসিক HTTP: স্ট্রিমিং রেসপন্স প্রক্রিয়া করার সহজ ক্লায়েন্ট
   - MCP: বিভিন্ন ধরনের মেসেজ প্রক্রিয়া করার জন্য মেসেজ হ্যান্ডলার সহ উন্নত ক্লায়েন্ট

- **প্রগ্রেস আপডেট:**
   - ক্লাসিক HTTP: প্রগ্রেস প্রধান রেসপন্স স্ট্রিমের অংশ
   - MCP: প্রগ্রেস আলাদা নোটিফিকেশন মেসেজ হিসেবে পাঠানো হয়, প্রধান রেসপন্স শেষে আসে

### সুপারিশসমূহ

ক্লাসিক স্ট্রিমিং (যেমন `/stream` এন্ডপয়েন্ট ব্যবহার করে) এবং MCP স্ট্রিমিংয়ের মধ্যে নির্বাচন করার সময় কিছু সুপারিশ:

- **সহজ স্ট্রিমিংয়ের জন্য:** ক্লাসিক HTTP স্ট্রিমিং সহজ এবং মৌলিক স্ট্রিমিং প্রয়োজনের জন্য যথেষ্ট।

- **জটিল, ইন্টারেক্টিভ অ্যাপ্লিকেশনের জন্য:** MCP স্ট্রিমিং কাঠামোবদ্ধ পদ্ধতি প্রদান করে, সমৃদ্ধ মেটাডেটা এবং নোটিফিকেশন ও চূড়ান্ত ফলাফলের পৃথকীকরণ সহ।

- **AI অ্যাপ্লিকেশনের জন্য:** MCP এর নোটিফিকেশন সিস্টেম দীর্ঘমেয়াদী AI টাস্কের জন্য বিশেষভাবে উপযোগী যেখানে ব্যবহারকারীকে প্রগ্রেস সম্পর্কে অবহিত রাখতে হয়।

## MCP-তে স্ট্রিমিং

এখন পর্যন্ত ক্লাসিক স্ট্রিমিং এবং MCP স্ট্রিমিংয়ের মধ্যে পার্থক্য এবং সুপারিশ দেখেছেন। চলুন বিস্তারিতভাবে দেখি MCP-তে স্ট্রিমিং কিভাবে ব্যবহার করবেন।

MCP ফ্রেমওয়ার্কের মধ্যে স্ট্রিমিং কিভাবে কাজ করে তা বোঝা জরুরি, যাতে দীর্ঘমেয়াদী অপারেশনের সময় ব্যবহারকারীদের রিয়েল-টাইম ফিডব্যাক প্রদান করা যায়।

MCP-তে স্ট্রিমিং মানে প্রধান রেসপন্স চাঙ্কে পাঠানো নয়, বরং টুল যখন অনুরোধ প্রক্রিয়া করছে তখন ক্লায়েন্টকে **নোটিফিকেশন** পাঠানো। এই নোটিফিকেশনগুলোতে প্রগ্রেস আপডেট, লগ বা অন্যান্য ইভেন্ট থাকতে পারে।

### কিভাবে কাজ করে

প্রধান ফলাফল এখনও একক রেসপন্স হিসেবে পাঠানো হয়। তবে, প্রক্রিয়াকরণের সময় আলাদা মেসেজ হিসেবে নোটিফিকেশন পাঠানো হয় এবং এর মাধ্যমে ক্লায়েন্টকে রিয়েল-টাইমে আপডেট করা হয়। ক্লায়েন্টকে এই নোটিফিকেশনগুলো হ্যান্ডেল এবং প্রদর্শন করতে সক্ষম হতে হবে।

## নোটিফিকেশন কী?

আমরা "নোটিফিকেশন" বলেছি, MCP প্রেক্ষাপটে এর অর্থ কী?

নোটিফিকেশন হল সার্ভার থেকে ক্লায়েন্টকে পাঠানো একটি মেসেজ যা দীর্ঘমেয়াদী অপারেশনের সময় প্রগ্রেস, অবস্থা বা অন্যান্য ইভেন্ট সম্পর্কে জানায়। নোটিফিকেশন স্বচ্ছতা এবং ব্যবহারকারীর অভিজ্ঞতা উন্নত করে।

উদাহরণস্বরূপ, ক্লায়েন্টকে সার্ভারের সাথে প্রাথমিক হ্যান্ডশেক সম্পন্ন হওয়ার পর একটি নোটিফিকেশন পাঠাতে হবে।

নোটিফিকেশন JSON মেসেজের মতো দেখতে হয়:

```json
{
  jsonrpc: "2.0";
  method: string;
  params?: {
    [key: string]: unknown;
  };
}
```

নোটিফিকেশন MCP-র একটি টপিকের অন্তর্গত যা ["Logging"](https://modelcontextprotocol.io/specification/draft/server/utilities/logging) নামে পরিচিত।

লগিং কাজ করার জন্য সার্ভারকে এটি একটি ফিচার/ক্ষমতা হিসেবে সক্রিয় করতে হবে, যেমন:

```json
{
  "capabilities": {
    "logging": {}
  }
}
```

> [!NOTE]
> ব্যবহৃত SDK অনুসারে, লগিং ডিফল্টরূপে সক্রিয় থাকতে পারে, অথবা আপনাকে সার্ভার কনফিগারেশনে এটি স্পষ্টভাবে সক্রিয় করতে হতে পারে।

নোটিফিকেশনের বিভিন্ন ধরণ রয়েছে:

| স্তর       | বর্ণনা                         | উদাহরণ ব্যবহারের ক্ষেত্র          |
|-----------|-------------------------------|---------------------------------|
| debug     | বিস্তারিত ডিবাগিং তথ্য          | ফাংশন এন্ট্রি/এক্সিট পয়েন্ট    |
| info      | সাধারণ তথ্যবহুল মেসেজ          | অপারেশন প্রগ্রেস আপডেট          |
| notice    | স্বাভাবিক কিন্তু গুরুত্বপূর্ণ ইভেন্ট | কনফিগারেশন পরিবর্তন             |
| warning   | সতর্কতা শর্ত                   | অব্যবহৃত ফিচার ব্যবহারের সতর্কতা |
| error     | ত্রুটি শর্ত                   | অপারেশন ব্যর্থতা                |
| critical  | গুরুতর শর্ত                   | সিস্টেম কম্পোনেন্ট ব্যর্থতা      |
| alert     | অবিলম্বে ব্যবস্থা নিতে হবে     | ডেটা করাপশন সনাক্তকরণ          |
| emergency | সিস্টেম ব্যবহার অযোগ্য          | সম্পূর্ণ সিস্টেম ব্যর্থতা        |

## MCP-তে নোটিফিকেশন বাস্তবায়ন

MCP-তে নোটিফিকেশন বাস্তবায়নের জন্য সার্ভার এবং ক্লায়েন্ট উভয়কেই রিয়েল-টাইম আপডেট হ্যান্ডেল করার জন্য প্রস্তুত করতে হবে। এর ফলে দীর্ঘমেয়াদী অপারেশনের সময় ব্যবহারকারীদের অবিলম্বে প্রতিক্রিয়া প্রদান সম্ভব হয়।

### সার্ভার-সাইড: নোটিফিকেশন পাঠানো

সার্ভার সাইড থেকে শুরু করা যাক। MCP-তে আপনি এমন টুল সংজ্ঞায়িত করেন যা অনুরোধ প্রক্রিয়াকরণের সময় নোটিফিকেশন পাঠাতে পারে। সার্ভার সাধারণত `ctx` নামে কনটেক্সট অবজেক্ট ব্যবহার করে ক্লায়েন্টকে মেসেজ পাঠায়।

### Python

```python
@mcp.tool(description="A tool that sends progress notifications")
async def process_files(message: str, ctx: Context) -> TextContent:
    await ctx.info("Processing file 1/3...")
    await ctx.info("Processing file 2/3...")
    await ctx.info("Processing file 3/3...")
    return TextContent(type="text", text=f"Done: {message}")
```

উপরের উদাহরণে, `process_files` টুল প্রতিটি ফাইল প্রক্রিয়াকরণের সময় ক্লায়েন্টকে তিনটি নোটিফিকেশন পাঠায়। `ctx.info()` মেথড তথ্যবহুল মেসেজ পাঠাতে ব্যবহৃত হয়।

অতিরিক্তভাবে, নোটিফিকেশন সক্ষম করতে, নিশ্চিত করুন আপনার সার্ভার স্ট্রিমিং ট্রান্সপোর্ট (যেমন `streamable-http`) ব্যবহার করছে এবং ক্লায়েন্ট নোটিফিকেশন প্রক্রিয়াকরণের জন্য মেসেজ হ্যান্ডলার বাস্তবায়ন করেছে। নিচে দেখানো হলো কিভাবে সার্ভার `streamable-http` ট্রান্সপোর্ট ব্যবহার করবে:

```python
mcp.run(transport="streamable-http")
```

### .NET

```csharp
[Tool("A tool that sends progress notifications")]
public async Task<TextContent> ProcessFiles(string message, ToolContext ctx)
{
    await ctx.Info("Processing file 1/3...");
    await ctx.Info("Processing file 2/3...");
    await ctx.Info("Processing file 3/3...");
    return new TextContent
    {
        Type = "text",
        Text = $"Done: {message}"
    };
}
```

এই .NET উদাহরণে, `ProcessFiles` টুল `Tool` অ্যাট্রিবিউট দ্বারা চিহ্নিত এবং প্রতিটি ফাইল প্রক্রিয়াকরণের সময় ক্লায়েন্টকে তিনটি নোটিফিকেশন পাঠায়। `ctx.Info()` মেথড তথ্যবহুল মেসেজ পাঠাতে ব্যবহৃত হয়।

আপনার .NET MCP সার্ভারে নোটিফিকেশন সক্ষম করতে, নিশ্চিত করুন আপনি স্ট্রিমিং ট্রান্সপোর্ট ব্যবহার করছেন:

```csharp
var builder = McpBuilder.Create();
await builder
    .UseStreamableHttp() // Enable streamable HTTP transport
    .Build()
    .RunAsync();
```

### ক্লায়েন্ট-সাইড: নোটিফিকেশন গ্রহণ

ক্লায়েন্টকে একটি মেসেজ হ্যান্ডলার বাস্তবায়ন করতে হবে যা আসা নোটিফিকেশন প্রক্রিয়া এবং প্রদর্শন করবে।

### Python

```python
async def message_handler(message):
    if isinstance(message, types.ServerNotification):
        print("NOTIFICATION:", message)
    else:
        print("SERVER MESSAGE:", message)

async with ClientSession(
   read_stream, 
   write_stream,
   logging_callback=logging_collector,
   message_handler=message_handler,
) as session:
```

উপরের কোডে, `message_handler` ফাংশন চেক করে আসা মেসেজ নোটিফিকেশন কিনা। যদি হয়, এটি নোটিফিকেশন প্রিন্ট করে; অন্যথায়, এটি সাধারণ সার্ভার মেসেজ হিসেবে প্রক্রিয়া করে। এছাড়াও লক্ষ্য করুন `ClientSession` কে `message_handler` দিয়ে ইনিশিয়ালাইজ করা হয়েছে যাতে আসা নোটিফিকেশন হ্যান্ডেল করা যায়।

### .NET

```csharp
// Define a message handler
void MessageHandler(IJsonRpcMessage message)
{
    if (message is ServerNotification notification)
    {
        Console.WriteLine($"NOTIFICATION: {notification}");
    }
    else
    {
        Console.WriteLine($"SERVER MESSAGE: {message}");
    }
}

// Create and use a client session with the message handler
var clientOptions = new ClientSessionOptions
{
    MessageHandler = MessageHandler,
    LoggingCallback = (level, message) => Console.WriteLine($"[{level}] {message}")
};

using var client = new ClientSession(readStream, writeStream, clientOptions);
await client.InitializeAsync();

// Now the client will process notifications through the MessageHandler
```

এই .NET উদাহরণে, `MessageHandler` ফাংশন চেক করে আসা মেসেজ নোটিফিকেশন কিনা। যদি হয়, এটি নোটিফিকেশন প্রিন্ট করে; অন্যথায়, এটি সাধারণ সার্ভার মেসেজ হিসেবে প্রক্রিয়া করে। `ClientSession` মেসেজ হ্যান্ডলার সহ `ClientSessionOptions` এর মাধ্যমে ইনিশিয়ালাইজ করা হয়েছে।

নোটিফিকেশন সক্ষম করতে, নিশ্চিত করুন আপনার সার্ভার স্ট্রিমিং ট্রান্সপোর্ট (যেমন `streamable-http`) ব্যবহার করছে এবং ক্লায়েন্ট নোটিফিকেশন প্রক্রিয়াকরণের জন্য মেসেজ হ্যান্ডলার বাস্তবায়ন করেছে।

## প্রগ্রেস নোটিফিকেশন এবং পরিস্থিতি

এই অংশে MCP-তে প্রগ্রেস নোটিফিকেশনের ধারণা, এর গুরুত্ব এবং Streamable HTTP ব্যবহার করে কিভাবে বাস্তবায়ন করবেন তা ব্যাখ্যা করা হয়েছে। এছাড়াও একটি ব্যবহারিক অ্যাসাইনমেন্ট রয়েছে যা আপনার বোঝাপড়া দৃঢ় করবে।

প্রগ্রেস নোটিফিকেশন হল সার্ভার থেকে ক্লায়েন্টকে দীর্ঘমেয়াদী অপারেশনের সময় পাঠানো রিয়েল-টাইম মেসেজ। পুরো প্রক্রিয়া শেষ হওয়ার জন্য অপেক্ষা না করে সার্ভার ক্লায়েন্টকে বর্তমান অবস্থা সম্পর্কে আপডেট রাখে। এটি স্বচ্ছতা, ব্যবহারকারীর অভিজ্ঞতা উন্নত করে এবং ডিবাগিং সহজ করে।

**উদাহরণ:**

```text

"Processing document 1/10"
"Processing document 2/10"
...
"Processing complete!"

```

### কেন প্রগ্রেস নোটিফিকেশন ব্যবহার করবেন?

প্রগ্রেস নোটিফিকেশন গুরুত্বপূর্ণ কয়েকটি কারণে:

- **ভাল ব্যবহারকারীর অভিজ্ঞতা:** কাজ চলাকালীন ব্যবহারকারীরা আপডেট দেখতে পায়, শুধু শেষে নয়।
- **রিয়েল-টাইম ফিডব্যাক:** ক্লায়েন্ট প্রগ্রেস বার বা লগ দেখাতে পারে, অ্যাপ্লিকেশনকে প্রতিক্রিয়াশীল করে তোলে।
- **সহজ ডিবাগিং ও মনিটরিং:** ডেভেলপার এবং ব্যবহারকারী দেখতে পারে কোন প্রক্রিয়া ধীর বা আটকে আছে।


### কেন আপগ্রেড করবেন?

SSE থেকে Streamable HTTP-তে আপগ্রেড করার দুটি গুরুত্বপূর্ণ কারণ রয়েছে:

- Streamable HTTP SSE-এর তুলনায় উন্নত স্কেলেবিলিটি, সামঞ্জস্য এবং সমৃদ্ধ নোটিফিকেশন সাপোর্ট প্রদান করে।
- এটি নতুন MCP অ্যাপ্লিকেশনের জন্য সুপারিশকৃত ট্রান্সপোর্ট।

### মাইগ্রেশন ধাপসমূহ

আপনার MCP অ্যাপ্লিকেশনগুলোতে SSE থেকে Streamable HTTP-তে মাইগ্রেট করার জন্য নিচের ধাপগুলো অনুসরণ করুন:

- **সার্ভার কোড আপডেট করুন** `mcp.run()`-এ `transport="streamable-http"` ব্যবহার করতে।
- **ক্লায়েন্ট কোড আপডেট করুন** SSE ক্লায়েন্টের পরিবর্তে `streamablehttp_client` ব্যবহার করতে।
- **ক্লায়েন্টে একটি মেসেজ হ্যান্ডলার ইমপ্লিমেন্ট করুন** যা নোটিফিকেশনগুলো প্রক্রিয়া করবে।
- **বিদ্যমান টুলস এবং ওয়ার্কফ্লো-এর সাথে সামঞ্জস্য পরীক্ষা করুন**।

### সামঞ্জস্য বজায় রাখা

মাইগ্রেশন চলাকালীন বিদ্যমান SSE ক্লায়েন্টগুলোর সাথে সামঞ্জস্য বজায় রাখা সুপারিশ করা হয়। কিছু কৌশল হলো:

- আপনি SSE এবং Streamable HTTP উভয়কেই সমর্থন করতে পারেন, আলাদা এন্ডপয়েন্টে উভয় ট্রান্সপোর্ট চালিয়ে।
- ধীরে ধীরে ক্লায়েন্টদের নতুন ট্রান্সপোর্টে মাইগ্রেট করুন।

### চ্যালেঞ্জসমূহ

মাইগ্রেশনের সময় নিম্নলিখিত চ্যালেঞ্জগুলো মোকাবেলা করতে হবে:

- নিশ্চিত করা যে সব ক্লায়েন্ট আপডেট হয়েছে
- নোটিফিকেশন ডেলিভারির পার্থক্যগুলো সামলানো

## নিরাপত্তা বিবেচনা

কোনো সার্ভার ইমপ্লিমেন্ট করার সময় নিরাপত্তা সর্বোচ্চ অগ্রাধিকার হওয়া উচিত, বিশেষ করে MCP-তে Streamable HTTP-এর মতো HTTP-ভিত্তিক ট্রান্সপোর্ট ব্যবহার করার সময়।

HTTP-ভিত্তিক ট্রান্সপোর্ট সহ MCP সার্ভার ইমপ্লিমেন্ট করার সময় নিরাপত্তা একটি গুরুত্বপূর্ণ বিষয়, যা বিভিন্ন আক্রমণ পদ্ধতি এবং সুরক্ষা ব্যবস্থা সম্পর্কে সতর্ক মনোযোগ দাবি করে।

### ওভারভিউ

HTTP-এর মাধ্যমে MCP সার্ভার প্রকাশ করার সময় নিরাপত্তা অত্যন্ত গুরুত্বপূর্ণ। Streamable HTTP নতুন আক্রমণ ক্ষেত্র তৈরি করে এবং সতর্ক কনফিগারেশন প্রয়োজন।

নিম্নলিখিত মূল নিরাপত্তা বিষয়গুলো বিবেচনা করুন:

- **Origin Header যাচাই**: DNS rebinding আক্রমণ প্রতিরোধে সবসময় `Origin` হেডার যাচাই করুন।
- **Localhost বাইনডিং**: স্থানীয় ডেভেলপমেন্টের জন্য সার্ভারগুলো `localhost`-এ বাইন্ড করুন যাতে সেগুলো পাবলিক ইন্টারনেটে প্রকাশ না পায়।
- **প্রমাণীকরণ**: প্রোডাকশনে API কী, OAuth ইত্যাদি ব্যবহার করে প্রমাণীকরণ ইমপ্লিমেন্ট করুন।
- **CORS**: অ্যাক্সেস সীমাবদ্ধ করতে Cross-Origin Resource Sharing (CORS) নীতি কনফিগার করুন।
- **HTTPS**: প্রোডাকশনে ট্রাফিক এনক্রিপ্ট করার জন্য HTTPS ব্যবহার করুন।

### সেরা অনুশীলনসমূহ

MCP স্ট্রিমিং সার্ভারে নিরাপত্তা ইমপ্লিমেন্ট করার সময় নিচের সেরা অনুশীলনগুলো অনুসরণ করুন:

- যাচাই ছাড়া কোনো ইনকামিং রিকোয়েস্ট বিশ্বাস করবেন না।
- সব অ্যাক্সেস এবং এরর লগ ও মনিটর করুন।
- নিরাপত্তা দুর্বলতা প্যাচ করার জন্য নিয়মিত ডিপেন্ডেন্সি আপডেট করুন।

### চ্যালেঞ্জসমূহ

MCP স্ট্রিমিং সার্ভারে নিরাপত্তা ইমপ্লিমেন্ট করার সময় আপনি কিছু চ্যালেঞ্জের মুখোমুখি হবেন:

- উন্নয়নের সহজতার সাথে নিরাপত্তার ভারসাম্য রক্ষা করা
- বিভিন্ন ক্লায়েন্ট পরিবেশের সাথে সামঞ্জস্য নিশ্চিত করা

### অ্যাসাইনমেন্ট: নিজস্ব স্ট্রিমিং MCP অ্যাপ তৈরি করুন

**পরিস্থিতি:**
একটি MCP সার্ভার এবং ক্লায়েন্ট তৈরি করুন যেখানে সার্ভার একটি আইটেমের তালিকা (যেমন ফাইল বা ডকুমেন্ট) প্রক্রিয়া করে এবং প্রতিটি আইটেম প্রক্রিয়াকরণের জন্য একটি নোটিফিকেশন পাঠায়। ক্লায়েন্ট প্রতিটি নোটিফিকেশন আসার সাথে সাথেই তা প্রদর্শন করবে।

**ধাপসমূহ:**

1. একটি সার্ভার টুল ইমপ্লিমেন্ট করুন যা একটি তালিকা প্রক্রিয়া করে এবং প্রতিটি আইটেমের জন্য নোটিফিকেশন পাঠায়।
2. একটি ক্লায়েন্ট ইমপ্লিমেন্ট করুন যার মেসেজ হ্যান্ডলার থাকবে যা রিয়েল টাইমে নোটিফিকেশন দেখাবে।
3. সার্ভার এবং ক্লায়েন্ট উভয় চালিয়ে আপনার ইমপ্লিমেন্টেশন পরীক্ষা করুন এবং নোটিফিকেশনগুলো পর্যবেক্ষণ করুন।

[Solution](./solution/README.md)

## আরও পড়াশোনা ও পরবর্তী ধাপ

MCP স্ট্রিমিং নিয়ে আপনার যাত্রা চালিয়ে যেতে এবং আরও উন্নত অ্যাপ্লিকেশন তৈরি করার জন্য এই অংশে অতিরিক্ত রিসোর্স এবং পরবর্তী ধাপের পরামর্শ দেওয়া হয়েছে।

### আরও পড়াশোনা

- [Microsoft: Introduction to HTTP Streaming](https://learn.microsoft.com/aspnet/core/fundamentals/http-requests?view=aspnetcore-8.0&WT.mc_id=%3Fwt.mc_id%3DMVP_452430#streaming)
- [Microsoft: Server-Sent Events (SSE)](https://learn.microsoft.com/azure/application-gateway/for-containers/server-sent-events?tabs=server-sent-events-gateway-api&WT.mc_id=%3Fwt.mc_id%3DMVP_452430)
- [Microsoft: CORS in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-8.0&WT.mc_id=%3Fwt.mc_id%3DMVP_452430)
- [Python requests: Streaming Requests](https://requests.readthedocs.io/en/latest/user/advanced/#streaming-requests)

### পরবর্তী ধাপ

- রিয়েল-টাইম অ্যানালিটিক্স, চ্যাট বা সহযোগিতামূলক এডিটিংয়ের জন্য স্ট্রিমিং ব্যবহার করে আরও উন্নত MCP টুল তৈরি করার চেষ্টা করুন।
- MCP স্ট্রিমিংকে ফ্রন্টএন্ড ফ্রেমওয়ার্ক (React, Vue ইত্যাদি) এর সাথে ইন্টিগ্রেট করে লাইভ UI আপডেট এক্সপ্লোর করুন।
- পরবর্তী: [Utilising AI Toolkit for VSCode](../07-aitk/README.md)

**অস্বীকৃতি**:  
এই নথিটি AI অনুবাদ সেবা [Co-op Translator](https://github.com/Azure/co-op-translator) ব্যবহার করে অনূদিত হয়েছে। আমরা যথাসাধ্য সঠিকতার চেষ্টা করি, তবে স্বয়ংক্রিয় অনুবাদে ত্রুটি বা অসঙ্গতি থাকতে পারে। মূল নথিটি তার নিজস্ব ভাষায়ই কর্তৃত্বপূর্ণ উৎস হিসেবে বিবেচিত হওয়া উচিত। গুরুত্বপূর্ণ তথ্যের জন্য পেশাদার মানব অনুবাদ গ্রহণ করার পরামর্শ দেওয়া হয়। এই অনুবাদের ব্যবহারে সৃষ্ট কোনো ভুল বোঝাবুঝি বা ভুল ব্যাখ্যার জন্য আমরা দায়ী নই।