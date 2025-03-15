# 签名过程示例

在进行API请求时，生成签名是确保请求安全性的重要步骤。以下是一个生成POST/GET请求签名的示例：

## 签名步骤

1. **准备请求参数**：
   - 将所有请求参数按字母顺序排序。

2. **构建待签名字符串**：
   - 将排序后的参数转出JSON，时间戳和参数之间用`&`连接。
   - 示例：`'1686796826&{"param1":"value1","param2":value2"}'`

3. **生成签名**：
   - 使用HMAC-SHA256算法对字符串进行加密。
   - 将生成的哈希值转换为十六进制字符串。

4. **添加签名到请求头**：
   - 将生成的签名添加到请求头中，通常使用`SIGNATURE`字段。

5. **请求**：
   - GET URL：使用 buildGetUrl 方法，将URL和查询参数结合
   - 示例：`param1=value1&param2=value2&...&`

## Java 签名步骤示例代码

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import okhttp3.*;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.time.Instant;
import java.util.Map;
import java.util.TreeMap;

public class Main {
    private static final String API_KEY = "B433BFF1CDE7450AA38A56BEAC690DD4";
    private static final String API_SECRET = "0285A2741D0E76E2E187260EB23E51851D48403A756333E7D0CF845406ABF3E8";

    public static void main(String[] args) throws Exception {
        OkHttpClient client = new OkHttpClient().newBuilder().build();
        MediaType mediaType = MediaType.parse("application/json");
        String timestamp = String.valueOf(Instant.now().getEpochSecond());

        // 示例：发送 POST 请求
        String urlPost = "https://example.com/api";  // POST 请求的 URL
        Map<String, Object> dataPost = Map.of("param1", 10000, "param2", "value");

        String postSignature = generateSignature(timestamp, dataPost, urlPost);

        RequestBody body = RequestBody.create(new Gson().toJson(dataPost), mediaType);
        Request requestPost = new Request.Builder()
                .url(urlPost)
                .method("POST", body)
                .addHeader("API-KEY", API_KEY)
                .addHeader("TIMESTAMP", timestamp)
                .addHeader("SIGNATURE", postSignature)
                .addHeader("Content-Type", "application/json")
                .build();
        Response responsePost = client.newCall(requestPost).execute();
        System.out.println("POST Response: " + responsePost.body().string());

        // 示例：发送 GET 请求
        String urlGet = "https://example.com/api";  // GET 请求的 URL
        Map<String, Object> dataGet = Map.of("param1", "value1", "param2", "value2");

        String getSignature = generateSignature(timestamp, dataGet, urlGet);

        String getUrlWithParams = buildGetUrl(urlGet, dataGet);

        Request requestGet = new Request.Builder()
                .url(getUrlWithParams)
                .method("GET", null)
                .addHeader("API-KEY", API_KEY)
                .addHeader("TIMESTAMP", timestamp)
                .addHeader("SIGNATURE", getSignature)
                .addHeader("Content-Type", "application/json")
                .build();
        Response responseGet = client.newCall(requestGet).execute();
        System.out.println("GET Response: " + responseGet.body().string());
    }

    // 统一生成签名方法
    private static String generateSignature(String timestamp, Map<String, Object> data, String url) throws Exception {
        StringBuilder message = new StringBuilder(timestamp);
        if (data != null && !data.isEmpty()) {
            // 使用 Gson 将请求参数转为 JSON，并排序
            TreeMap<String, Object> sortedData = new TreeMap<>(data);
            Gson gson = new GsonBuilder().disableHtmlEscaping().create();
            String json_data = gson.toJson(sortedData);
            message.append("&").append(json_data);
        }
        // 使用 HMAC-SHA256 算法生成签名
        return encodeHmacSHA256(message.toString(), API_SECRET);
    }

    // 构建 GET 请求的查询字符串
    private static String buildQueryString(Map<String, Object> params) {
        StringBuilder queryString = new StringBuilder();
        for (Map.Entry<String, Object> entry : params.entrySet()) {
            if (queryString.length() > 0) {
                queryString.append("&");
            }
            queryString.append(entry.getKey())
                    .append("=")
                    .append(entry.getValue());
        }
        return queryString.toString();
    }

    // 构建完整的 GET URL（带查询参数）
    private static String buildGetUrl(String url, Map<String, Object> params) {
        StringBuilder getUrl = new StringBuilder(url);
        if (params != null && !params.isEmpty()) {
            getUrl.append("?");
            getUrl.append(buildQueryString(params));
        }
        return getUrl.toString();
    }

    // HMAC-SHA256 加密算法
    private static String encodeHmacSHA256(String data, String key) throws Exception {
        Mac sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(StandardCharsets.UTF_8), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        byte[] bytes = sha256_HMAC.doFinal(data.getBytes(StandardCharsets.UTF_8));
        StringBuilder hash = new StringBuilder();
        for (byte b : bytes) {
            String hex = Integer.toHexString(0xff & b);
            if (hex.length() == 1) hash.append('0');
            hash.append(hex);
        }
        return hash.toString();
    }
}

