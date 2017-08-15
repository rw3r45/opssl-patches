# Opera-SSL patches

This repository contains patches for upgrading the OpenSSL library bundled with libopeay, a part of Opera Browser.
They were *(barely)* tested on 64bit GNU/Linux and most likely don't work on other platforms, but feel free to try.

# These patches are dirty, terrible and you probably don't want to use them.

The Opera team has put a tremendous amount of work towards optimizing and securing their software. A proper patch aiming at upgrading bundled OpenSSL would take weeks, maybe months to make. This was made over a weekend, mostly using automated tools. These tools made terrible, terrible mistakes. The ones that made the compiler unhappy were fixed, many probably still remain. This patch probably **introduces more issues than respective OpenSSL versions fix**. You have been warned.

## Contents

- **OPERA_PATCH_OPENSSL_1_0_0q**: This is the last version of OpenSSL 1.0.0 branch before a major code refactoring that broke absolutely everything in terms of automerging with Opera code. This is your safest bet if you want to try compiling for other platforms.
- **OPERA_PATCH_OPENSSL_1_0_0t**: Most recent version of OpenSSL 1.0.0 branch.

## The future

I will probably continue this to OpenSSL 1.0.1, possibly 1.0.2; 1.1.0 or later seems highly unlikely. When (if) I'm done with that, there's still the challenge of implementing the new cipher suites in Operas' libssl, which probably won't happen either.

## Disclaimer

THIS SOFTWARE IS PROVIDED ``AS IS'' AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE AUTHORS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.