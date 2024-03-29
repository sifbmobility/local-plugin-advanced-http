# Customized

# v3.3.1

local_plugins\cordova-plugin-advanced-http\3.3.1\cordova-plugin-advanced-http\src\android\com\silkimen\cordovahttp\CordovaHttpBase.java

- Removed request.acceptCharset("UTF-8");

====
File changed (apps/showroom-mb/local_plugins/cordova-plugin-advanced-http/3.3.1/cordova-plugin-advanced-http/src/android/com/silkimen/cordovahttp/CordovaServerTrust.java)
<Remove this>
this.tlsConfiguration.setHostnameVerifier(this.noOpVerifier);
<Change to>
this.tlsConfiguration.setHostnameVerifier(null);

<Remove this>
this.noOpVerifier = new HostnameVerifier() {
public boolean verify(String hostname, SSLSession session) {
return true;
}
};

====
File changed (apps/showroom-mb/local_plugins/cordova-plugin-advanced-http/3.3.1/cordova-plugin-advanced-http/src/android/com/silkimen/http/TLSConfiguration.java)
Line 52:

- From
  SSLContext context = SSLContext.getInstance("TLS");
- To
  SSLContext context = SSLContext.getInstance("TLSv1.2");

<Remove this>
private final HostnameVerifier noOpVerifier;

====
File changed (apps/showroom-mb/local_plugins/cordova-plugin-advanced-http/3.3.1/cordova-plugin-advanced-http/src/android/com/silkimen/cordovahttp/CordovaClientAuth.java)

Line 75:

- From
  private void loadFromBuffer() {
  try {
  KeyStore keyStore = KeyStore.getInstance("PKCS12");
  String keyManagerFactoryAlgorithm = KeyManagerFactory.getDefaultAlgorithm();
  KeyManagerFactory keyManagerFactory = KeyManagerFactory.getInstance(keyManagerFactoryAlgorithm);
  ByteArrayInputStream stream = new ByteArrayInputStream(this.rawPkcs);

        keyStore.load(stream, this.pkcsPassword.toCharArray());
        keyManagerFactory.init(keyStore, this.pkcsPassword.toCharArray());

        this.tlsConfiguration.setKeyManagers(keyManagerFactory.getKeyManagers());
        this.callbackContext.success();
      } catch (Exception e) {
        Log.e(TAG, "Couldn't load given PKCS12 container for authentication", e);
        this.callbackContext.error("Couldn't load given PKCS12 container for authentication");
      }

  }

- To
  private void loadFromBuffer() {
  try {
  KeyStore keyStore = KeyStore.getInstance("PKCS12");
  String keyManagerFactoryAlgorithm = KeyManagerFactory.getDefaultAlgorithm();
  KeyManagerFactory keyManagerFactory = KeyManagerFactory.getInstance(keyManagerFactoryAlgorithm);
  ByteArrayInputStream stream = new ByteArrayInputStream(this.rawPkcs);

        keyStore.load(stream, this.pkcsPassword.toCharArray());
        keyManagerFactory.init(keyStore, this.pkcsPassword.toCharArray());

        this.tlsConfiguration.setKeyManagers(keyManagerFactory.getKeyManagers());
        this.callbackContext.success();
      } catch (NoSuchAlgorithmException | KeyStoreException e) {
        Log.e(TAG, "Couldn't load given PKCS12 container for authentication", e);
        this.callbackContext.error("Couldn't load given PKCS12 container for authentication");
      } catch (UnrecoverableKeyException e) {
        throw new RuntimeException(e);
      } catch (CertificateException e) {
        throw new RuntimeException(e);
      } catch (IOException e) {
        throw new RuntimeException(e);
      }

  }

# Redmine #19928 - Jenn Hong

affected path: showroom-nx/apps/showroom-mb/local_plugins/cordova-plugin-advanced-http/3.3.1/cordova-plugin-advanced-http/src/android/com/silkimen/http/HttpRequest.java

===========

Line 25 - Add new import for able to use Log class from the Android framework

import android.util.Log;

Line 683 - Add logging in catch block for error occurred

From-
catch (IOException e) {
// Ignored
}

To -
catch (IOException e) {
Log.e("Error closing resource", "An error occurred while closing the resource", e);
}

===========

# Installation

If your platforms having the existing plugin, remove it first. Make sure platforms & plugins do not have the plugins.

- Remove plugin
  cordova plugin rm cordova-plugin-advanced-http

- Add plugin
  cordova plugin add ./apps/showroom-mb/local_plugins/cordova-plugin-advanced-http/3.3.1/cordova-plugin-advanced-http
