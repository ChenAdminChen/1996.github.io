---
title: spring-boot-feign
date: 2019.10.28 14:02:34
tags: 
    - spring-boot
    - feign
    
categories: spring-boot

---


## spring-boot-feign ssl认证身份

### 1. feign使用的依赖包

```kotlin
   implementation("org.springframework.cloud:spring-cloud-starter-openfeign")
   testImplementation("org.springframework.boot:spring-boot-starter-test")

```

### 2. 代码如下

```yaml
wx-pay:
  mch_id: 1r5gfs90rtr61trw
  jks: /home/work/file/apiclient_cert.p12
  server_alias: tenpay certificate

```

```kotlin
import feign.Client
import feign.Feign
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration
import javax.net.ssl.SSLSocketFactory

// defined feign server
@Configuration
class CustomFeignConfiguration {
    @Bean
    fun feignBuilder(sSLSocketFactory: SSLSocketFactory): Feign.Builder {
        val trustSSLSockets = Client.Default(sSLSocketFactory, null)
        return Feign.builder().client(trustSSLSockets)
    }
}
```

//feign use ssl jks
```kotlin

import org.springframework.beans.factory.annotation.Value
import org.springframework.stereotype.Component
import java.io.IOException
import java.io.InputStream
import java.net.InetAddress
import java.net.Socket
import java.security.KeyStore
import java.security.Principal
import java.security.PrivateKey
import java.security.SecureRandom
import java.security.cert.X509Certificate
import java.util.Arrays
import javax.net.ssl.*
import java.io.FileInputStream

/**
    @mch_id: ssl password
    @jks: jks file
    @serverAlias: jks alias
*/ 
@Component
class TrustingSSLSocketFactory constructor(@Value("\${wx-pay.mch_id}")  val mch_id: String, @Value("\${wx-pay.jks}")  val jks: String, @Value("\${wx-pay.server_alias}")  val serverAlias: String) : SSLSocketFactory(), X509TrustManager, X509KeyManager {
    private val delegate: SSLSocketFactory
    private val privateKey: PrivateKey?
    private val certificateChain: Array<X509Certificate>?

    init {
        try {
            val sc = SSLContext.getInstance("SSL")
            sc.init(arrayOf<KeyManager>(this), arrayOf<TrustManager>(this), SecureRandom())
            this.delegate = sc.socketFactory
        } catch (e: Exception) {
            throw RuntimeException(e)
        }

        if (serverAlias.isEmpty()) {
            this.privateKey = null
            this.certificateChain = null
        } else {
            try {

                val fileInputStream = FileInputStream(jks)

//                val keyStore = loadKeyStore(TrustingSSLSocketFactory::class.java.getResourceAsStream(jks), mch_id)
                val keyStore = loadKeyStore(fileInputStream, mch_id)

                this.privateKey = keyStore.getKey(serverAlias, mch_id.toCharArray()) as PrivateKey

                val rawChain = keyStore.getCertificateChain(serverAlias)
                this.certificateChain = Arrays.copyOf(rawChain, rawChain.size, Array<X509Certificate>::class.java)
            } catch (e: Exception) {
                throw RuntimeException(e)
            }
        }
    }

    companion object {

        private val ENABLED_CIPHER_SUITES = arrayOf("TLS_RSA_WITH_AES_256_CBC_SHA")
//        //        private val sslSocketFactories = LinkedHashMap<String, SSLSocketFactory>()
//        private val KEYSTORE_PASSWORD = "1549064361".toCharArray()

//        fun get(): SSLSocketFactory {
//            return get("", "")
//        }

        //        @Synchronized
//        operator fun get(serverAlias: String, cert: String): SSLSocketFactory {
//
//            if (!sslSocketFactories.containsKey(serverAlias)) {
//                sslSocketFactories[serverAlias] = TrustingSSLSocketFactory(serverAlias)
//            }
//
//            return sslSocketFactories[serverAlias]!!
//        }
//
        internal fun setEnabledCipherSuites(socket: Socket): Socket {
            SSLSocket::class.java.cast(socket).enabledCipherSuites = ENABLED_CIPHER_SUITES
            return socket
        }

        @Throws(IOException::class)
        private fun loadKeyStore(inputStream: InputStream, password: String): KeyStore {
            try {
                val keyStore = KeyStore.getInstance("PKCS12")
                keyStore.load(inputStream, password.toCharArray())
                return keyStore
            } catch (e: Exception) {
                throw RuntimeException(e)
            } finally {
                inputStream.close()
            }
        }
    }

    override fun getDefaultCipherSuites(): Array<String> {
        return ENABLED_CIPHER_SUITES
    }

    override fun getSupportedCipherSuites(): Array<String> {
        return ENABLED_CIPHER_SUITES
    }

    @Throws(IOException::class)
    override fun createSocket(s: Socket, host: String, port: Int, autoClose: Boolean): Socket {
        return setEnabledCipherSuites(delegate.createSocket(s, host, port, autoClose))
    }

    @Throws(IOException::class)
    override fun createSocket(host: String, port: Int): Socket {
        return setEnabledCipherSuites(delegate.createSocket(host, port))
    }

    @Throws(IOException::class)
    override fun createSocket(host: InetAddress, port: Int): Socket {
        return setEnabledCipherSuites(delegate.createSocket(host, port))
    }

    @Throws(IOException::class)
    override fun createSocket(host: String, port: Int, localHost: InetAddress, localPort: Int): Socket {
        return setEnabledCipherSuites(delegate.createSocket(host, port, localHost, localPort))
    }

    @Throws(IOException::class)
    override fun createSocket(address: InetAddress, port: Int, localAddress: InetAddress, localPort: Int): Socket {
        return setEnabledCipherSuites(delegate.createSocket(address, port, localAddress, localPort))
    }

    override fun getAcceptedIssuers(): Array<X509Certificate>? {
        return null
    }

    override fun checkClientTrusted(certs: Array<X509Certificate>, authType: String) {}

    override fun checkServerTrusted(certs: Array<X509Certificate>, authType: String) {}

    override fun getClientAliases(keyType: String, issuers: Array<Principal>): Array<String>? {
        return null
    }

    override fun chooseClientAlias(keyType: Array<String>, issuers: Array<Principal>, socket: Socket): String? {
        return "null"
    }

    override fun getServerAliases(keyType: String, issuers: Array<Principal>): Array<String>? {
        return null
    }

    override fun chooseServerAlias(keyType: String, issuers: Array<Principal>, socket: Socket): String {
        return serverAlias
    }

    override fun getCertificateChain(alias: String): Array<X509Certificate>? {
        return certificateChain
    }

    override fun getPrivateKey(alias: String): PrivateKey? {
        return privateKey
    }


}

```