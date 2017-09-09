# Opera-SSL patches

This repository contains various patches dealing with SSL/TLS components of Opera Browser.
They were *(barely)* tested on 64bit GNU/Linux and most likely don't work on other platforms, but feel free to try.

# These patches are dirty, terrible and you probably don't want to use them.

The Opera team has put a tremendous amount of work towards optimizing and securing their software. A proper patch aiming at upgrading bundled OpenSSL would take weeks, maybe months to make. This was made over a weekend, mostly using automated tools. These tools made terrible, terrible mistakes. The ones that made the compiler unhappy were fixed, many probably still remain. This patch probably **introduces more issues than respective OpenSSL versions fix**. You have been warned.

Thanks to Phony Welder for debugging the 1.0.0 and 1.0.1 branch versions on Windows. They *shloud* compile now.

## Contents

### ssl
Stuff here contains patches for upgrading the OpenSSL library bundled with libopeay.

- **OPERA_PATCH_OPENSSL_1_0_0q**: This is the last version of OpenSSL 1.0.0 branch before a major code refactoring that broke absolutely everything in terms of automerging with Opera code. This is your safest bet if you want to try compiling for other platforms.
- **OPERA_PATCH_OPENSSL_1_0_0t**: Most recent version of OpenSSL 1.0.0 branch.
- **OPERA_PATCH_OPENSSL_1_0_1u**: Most recent version of OpenSSL 1.0.1 branch.
- **OPERA_PATCH_OPENSSL_1_0_2l**: Most recent version of OpenSSL 1.0.2 branch. This largely increases the amount of code because I'm lazy which might or might not translate to an increased attack surface.

### ciphers
Stuff here disables some weak and insecure ciphers that Opera supports. These patches are independent from each other and from the OpenSSL upgrade ones.

- **PatchRemoveInsecure**: Removes support for TLS_RSA_WITH_RC4_128_SHA and TLS_RSA_WITH_RC4_128_MD5.
- **PatchRemoveWeak**: Removes support for TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA, TLS_DH_DSS_WITH_3DES_EDE_CBC_SHA, TLS_DHE_RSA_WITH_3DES_EDE_CBC_SHA, TLS_DH_RSA_WITH_3DES_EDE_CBC_SHA and TLS_RSA_WITH_3DES_EDE_CBC_SHA

## The future

The OpenSSL codebase is large and its code distribution and internal APIs can change quite a lot even between minor versions, because of that I almost definitively won't be working on an 1.1.0+ patch.
OpenSSL 1.0.2 brings some new ciphers that Opera could definitely use and adding new ciphers is rather straightforward, a list of starting points would be `cipher_list.h`, `cipher_struct.h`, `external/openssl/genciph.cpp` and `base/sslenum.h` in `libssl` module (if you don't spend your evenings memorizing cipher suite codes, `openssl ciphers -V` provides the needed hexes). And the first logical task in my opinion, would be getting the AES_GCM (i.e. TLS_RSA_WITH_AES_128_GCM_SHA256 and TLS_RSA_WITH_AES_128_GCM_SHA384) cipher to work, [which works a bit differently](https://www.openssl.org/docs/man1.0.2/crypto/EVP_CipherInit.html#GCM-mode) than AES_CBC. Meaning it doesn't work. It might be due to the flow changes of the GCM itself, and is defninitely due to the switch from hashing the MAC to using [AEAD](https://tools.ietf.org/html/rfc5246#section-6.2.3.3) provided by GCM. Anyone interested in fixing the opera cipher flow I leave with this:
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout test.key -out test.crt
openssl s_server -key test.key -cert test.crt -accept 44330 -www -debug -msg -state -cipher 'AES128-GCM-SHA256'
```
You might need to remove `opssl6.dat` in profile to refresh the cipherlist Opera sends during client hello (don't ask me why) and enable tls v1.2 in `opera:config`. When done correctly you should expect a connection error 20 (transaction error) instead of 40 (no common ciphers) when connecting to the above openssl server, or just remove the cipher param and watch the reported client cipher list for any new ciphers that you've added. Good luck, I guess.


## Disclaimer

THIS SOFTWARE IS PROVIDED ``AS IS'' AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE AUTHORS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.