# Xamarin SDK for NCMB

ニフクラ mobile backendのXamarin用SDKです。

## インストール

コードをダウンロードして、自分のXamarinプロジェクトに取り込んでください。

**注意**

- ネームスペースが `ncmb_xamarin` になっていますので、適宜修正してください
- 依存ライブラリとしてJSON.NETを追加してください

## 使い方

### 初期化

```cs
var ncmb = new NCMB("ea5...265", "fe3...615");
```

### データストア

#### 保存

```cs
// データストアの操作
var hello = ncmb.Object("Hello");
hello.set("message", "Hello world");
hello.set("number", 100);
hello.set("bol", true);
var ary = new JArray();
ary.Add("test1");
ary.Add("test2");
hello.set("array", ary);
var obj = new JObject();
obj["test1"] = "Hello";
obj["test2"] = 100;
hello.set("obj", obj);
hello.set("time", DateTime.Now);
hello.save();
```

#### データのアクセス

返却値は JToken なので、必要な型にキャストしてください。

**文字列型の場合**

```cs
(string) hello.get("objectId")
```

**配列の場合**

```cs
var ary = (JArray) hello.get("array");
```

#### 検索

```cs
// 文字列、数字の検索
var query = ncmb.Query("Hello");
query.equalTo("message", "Test message").equalTo("number", 501);

var results = query.find();
Console.WriteLine(results[0].get("objectId"));

// 配列を検索
query.inString("message", new JArray("Test message"));
var results2 = query.find();
Console.WriteLine(results2[0].get("objectId"));

// 数値を使った検索
query.greaterThan("number", 500);
var results3 = query.find();
Console.WriteLine(results3[0].get("objectId"));

// 日付を使った検索
var query2 = ncmb.Query("Hello");
query2.greaterThan("time", DateTime.Parse("2020-07-10T08:40:00"));
var results4 = query2.find();
Console.WriteLine(results4[0].get("objectId"));
```

**その他のオペランド**

- equalTo(string name, object value)
- notEqualTo(string name, object value)
- lessThan(string name, object value)
- lessThanOrEqualTo(string name, object value)
- greaterThan(string name, object value)
- greaterThanOrEqualTo(string name, object value)
- inString(string name, object value)
- notInString(string name, object value)
- exists(string name, bool value = true)
- regularExpressionTo(string name, object value)
- inArray(string name, object value)
- notInArray(string name, object value)
- allInArray(string name, object value)

## ライセンス

MIT License

