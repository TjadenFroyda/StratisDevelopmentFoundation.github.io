---
title:  "Signing a message and Verifying a message"
date:   2018-03-09 16:16:01 -0600
categories: code_sample
post_importance: 3
---

# Signing a message and Verifying a message

If you have the private keys to an address, you can sign a message from that address. This produces a string.

Another person can take that string, and the address, and the message. They can verify the message was signed by the given address.

This is a way for someone who has the private keys to an address to prove they have them, without doing a transaction.

This is useful in certain authentication scenarios. For example, you could make signing a message from a Stratis address that has at least 100 coins grant access to a Stratis investor group. Or prove that you were the one who made a payment. Or prove that you have access to enough funds to make a large payment.

```cs
// somewhere up above, 'using NBitcoin;'

public static string GetMessageSignature(string message, string privateKeyAsString)
{
    BitcoinSecret sourcePrivateKey = new BitcoinSecret(privateKeyAsString, Network.StratisMain);
    return sourcePrivateKey.PrivateKey.SignMessage(message);
}

public static bool VerifySignedMessage(string message, string signature, string address)
{
    var stratisAddress = new BitcoinPubKeyAddress(address, Network.StratisMain);
    return stratisAddress.VerifyMessage(message, signature);
}
```

An article on how to get the private key string is needed. (dumping from a qt wallet, or from a c# code generated wallet)

An article on how to sign a message from the UI of the QT wallet is needed.

## Security Concerns

Signing a message like this does not expose your private keys. However, you should be careful how you are passing your private key as a string to the function. If you hardcode the private key into your program, the code now contains the password to your coins at the assoicated address.

There are no security concerns with verifying a signed message, as it does not involve private keys.