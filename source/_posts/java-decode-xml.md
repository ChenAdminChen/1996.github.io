---
title: kotlin(java)-decode-xml
date: 2019.10.28 14:02:34
tags: 
    - kotlin
    - xml
    
categories: java

---


## xml格式的数据转换成对象形式

### 1. xml解析使用的依赖包

```kotlin
    implementation("org.jetbrains.kotlin:kotlin-reflect")
    implementation("org.jetbrains.kotlin:kotlin-stdlib-jdk8")

    implementation("com.fasterxml.jackson.dataformat:jackson-dataformat-xml:2.10.0.pr1")
    implementation("com.fasterxml.jackson.core:jackson-core:2.10.0.pr1")
    implementation("com.fasterxml.jackson.module:jackson-module-kotlin:2.10.0.pr1")

```

### 2. 解析代码如下

```kotlin
import com.fasterxml.jackson.dataformat.xml.XmlMapper
import com.fasterxml.jackson.module.kotlin.registerKotlinModule

   val xml = """<xml>
   <appid><![CDATA[wx2421b1c4370ec43b]]></appid>
   <mch_id><![CDATA[10000100]]></mch_id>
   <nonce_str><![CDATA[TeqClE3i0mvn3DrK]]></nonce_str>
   <out_refund_no_0><![CDATA[1415701182]]></out_refund_no_0>
   <out_trade_no><![CDATA[1415757673]]></out_trade_no>
   <refund_count>1</refund_count>
   <refund_fee_0>1</refund_fee_0>
   <refund_id_0><![CDATA[2008450740201411110000174436]]></refund_id_0>
   <refund_status_0><![CDATA[PROCESSING]]></refund_status_0>
   <refund_fee_1>1</refund_fee_1>
   <refund_id_1><![CDATA[2008450740201411110000174436]]></refund_id_1>
   <refund_status_1><![CDATA[PROCESSING]]></refund_status_1>
   <result_code><![CDATA[SUCCESS]]></result_code>
   <return_code><![CDATA[SUCCESS]]></return_code>
   <return_msg><![CDATA[OK]]></return_msg>
   <sign><![CDATA[1F2841558E233C33ABA71A961D27561C]]></sign>
   <transaction_id><![CDATA[1008450740201411110005820873]]></transaction_id>
   <coupon_type_0_0><![CDATA[1008450740201411110005820873]]></coupon_type_0_0>
   <single_coupon_refund_fee_0_0><![CDATA[1008450740201411110005820873]]></single_coupon_refund_fee_0_0>
   <coupon_type_0_1><![CDATA[1008450740201411110005820873000____]]></coupon_type_0_1>
   <single_coupon_refund_fee_0_1><![CDATA[1008450740201411110005820873]]></single_coupon_refund_fee_0_1>
   <coupon_type_1_1><![CDATA[1008450740201411110005820873]]></coupon_type_1_1>
   <single_coupon_refund_fee_1_1><![CDATA[1008450740201411110005820873]]></single_coupon_refund_fee_1_1>
</xml>"""
// XmlMapper().registerKotlinModule() 需要注入（否则@JsonAlias不生效）
val xmlMapper = XmlMapper().registerKotlinModule()
val xmlMapper = XmlMapper()
var wxPayOrderQueryResult = xmlMapper.readValue(result, WxPayOrderResult::class.java)

```

### 3. 实体类代码如下：

```kotlin

@XmlRootElement(name = "xml")
@JacksonXmlRootElement(localName = "xml")
//@JsonSerialize(include = JsonSerialize.Inclusion.NON_NULL)
@JsonInclude(JsonInclude.Include.NON_NULL)
@JsonIgnoreProperties(ignoreUnknown = true)
data class WxPayOrderResult(
        @JsonAlias("return_code")
        var returnCode: String? = null,

        @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)
        var appid: String? = null,
        @JsonAlias("return_msg")
        var returnMsg: String? = null,
        @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)
        @JsonAlias("mch_id")
        var mchId: String? = null,
        @JsonAlias("nonce_str")
        var nonce: String? = null,

        var sign: String? = null,
        @JsonAlias("result_code")
        var resultCode: String? = null,
        @JsonAlias("err_code")
        var errCode: String? = null,
        @JsonAlias("err_code_des")
        var errCodeDes: String? = null,
        @JsonAlias("device_info")
        var deviceInfo: String? = null,
        var openid: String? = null,
        @JsonAlias("trade_type")
        var tradeType: String? = null,
        @JsonAlias("trade_state")
        var tradeState: String? = null,
        @JsonAlias("bank_type")
        var bankType: String? = null,
        @JsonAlias("total_fee")
        var totalFee: String? = null,
        @JsonAlias("fee_type")
        var feeType: String? = null,
        @JsonAlias("cash_fee")
        var cashFee: String? = null,
        @JsonAlias("cash_fee_type")  // `cash_fee_type`xml中属性名字
        var cashFeeType: String? = null,
        @JsonAlias("coupon_fee")
        var couponFee: String? = null,
        @JsonAlias("settlement_total_fee")
        var settlementTotalFee: String? = null,
        @JsonAlias("coupon_count")
        var couponCount: String? = null,
        @JsonAlias("transaction_id")
        var transactionId: String? = null,
        @JsonAlias("out_trade_no")
        var outTradeNo: String? = null,
        var attach: String? = null,
        @JsonAlias("time_end")
        var timeEnd: String? = null,
        @JsonAlias("trade_state_desc")
        var tradeStateDesc: String? = null,
        @JsonAlias("prepay_id")
        var prepayId: String? = null
) {
    @JsonProperty("is_subscribe")
    var subscribe: String? = null

}

```