# Authentication Headers
This is a Python library for the generation of email authentication headers.

## Authentication
The library can perform DKIM, SPF, and DMARC validation, and the results are packaged into the Authentication-Results header.

```
authenticate_message(message, "example.com", ip='192.168.50.81', mail_from="test.com", helo="domain.of.sender.net")

Authentication-Results: example.com; spf=none smtp.helo=domain.of.sender.net smtp.mailfrom=test.com; 
    dkim=pass header.d=valimail.com; dmarc=pass header.from=valimail.com
````

## Signature
The library can DKIM and ARC sign messages and output the corresponding signature headers.

```
sign_message(msg, b"dummy", b"example.org", privkey, b"from:to:date:subject:mime-version:arc-authentication-results".split(b':'), sig='ARC', auth_res=auth_res, timestamp="12345")

ARC-Seal: i=1; cv=none; a=rsa-sha256; d=example.org; s=dummy; t=12345; 
    b=FWOEyeRJ8YiqKt9x9GaZF62z/iy9i2606XLlnLC+Mfzf+8M92eWPPb50Pa+9d1iMwVRVeE
     8Rsdh6a7t+on2vLqBzFCuhA48AyQBVOMf4YgYKIxYbVHa5TD7GUOGSNCse8PGblJTcogmTL7
     FhApk4DJZQkuE4EWrMRMpzfxG24l4=
ARC-Message-Signature: i=1; a=rsa-sha256; c=relaxed/relaxed; d=example.org; s=dummy; t=12345; 
    h=from : to : date : subject : mime-version : arc-authentication-results; 
    bh=KWSe46TZKCcDbH4klJPo+tjk5LWJnVRlP5pvjXFZYLQ=; 
    b=LNev0+5hTRq5x+38IWMxbyZBXxZS6Ddacbul1XE7lEBKDXxh9MUvdGvCqdDoSSlUmJyx/s
     PLfucMfmftarx1xVIRPJeUrtuOZuUdQMPVpQcfQJ9pUfE1TG1KS4E2suCz3TF7uxu5OjaP21
     mjquuQP5lQe2fsnwBjBgVFcsSAwPw=
ARC-Authentication-Results: i=1; lists.example.org; spf=pass smtp.mfrom=jqd@d1.example; 
    dkim=pass (1024-bit key) header.i=@d1.example; dmarc=pass
```
