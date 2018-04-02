---
layout: default
title:  "Mnemonics for fun and profit"
date:   2018-04-02
---

Pssst. You want mnemonics? Oh yea we got mnemonics. We got all kinds of mnemonics.

Mnemonics are human readable phrases which can be used to create Bitcoin Cash keypairs for signing transactions, for generating addresses and for signing/verifying messages. Mnemonics can be created from sources of randomness and are much more secure than having a user think of a phrase or password.

BITBOX offers a `Mnemonic` class w/ a suite of methods for generating and validating [BIP39 mnemonics](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) in 8 languages including English, Chinese (traditional/simplified), Korean, Japanese, Spanish, Italian and French. Let's dig in.

## Generating mnemonics

Remember that mnemonics are a human readabe representation of underlying randomness or entropy. This entropy can be from 16 bytes up to 32 bytes. The lower the amount of entropy the less strong and secure your mnemonic will be.

Depending on the number of bytes of entropy you use mnemonics can be in phrases of 12, 15, 18, 21 and 24 words for 128/16, 160/20, 192/24, 224/28 and 256/32 bits/bytes respectively.

### generateMnemonic

The most straightforward way to create a mnemonic is `generateMnemonic`. It takes as it's 1st argument the number of bytes to use and the 2nd argument is an optional wordlist. By default BITBOX will create english mnemonics.

#### mnemonicWordLists

A quick sidenote about `mnemonicWordLists`. As mentioned above BITBOX supports 8 languages for mnemonics. Any methods in the `Mnemonic` class which take a 2nd optional wordlists argument will get their 2048 wordlist from `mnemonicWordLists` which returns an object w keys for each supported language.

```js
BITBOX.Mnemonic.mnemonicWordLists();
 // {
 //   chinese_simplified: [...],
 //   chinese_traditional: [...],
 //   english: [...],
 //   french: [...],
 //   italian: [...],
 //   japanese: [...],
 //   korean: [...],
 //   spanish: [...]
 // }

let frenchWordList = BITBOX.Mnemonic.mnemonicWordLists().french;
// [ 'abaisser',
//   'abandon',
//   'abdiquer',
//   'abeille',
//   'abolir',
//   'aborder',
//   'aboutir',
//   'aboyer',
//   'abrasif',
//   ...
```

[More info on mnemonicWordLists](https://www.bitbox.earth/bitboxcli/mnemonic#mnemonicWordLists)

Now lets generate some mnemonics in different lengths and languages.

```js
// generate 12 english word mnemonic
BITBOX.Mnemonic.generateMnemonic(128);
// boil lonely casino manage habit where total glory muffin name limit mansion

// generate 15 english word mnemonic
BITBOX.Mnemonic.generateMnemonic(160);
// steak prevent estate save dance design close noise cheap season among train sleep ketchup gas

// generate 18 french word mnemonic
BITBOX.Mnemonic.generateMnemonic(192, BITBOX.Mnemonic.mnemonicWordLists().french);
// germe tissu avide raconter griffure fureur vivipare brusque famille tituber cuillère burin rythme moulin éolien banquier lubie remuer

// generate 21 spanish word mnemonic
BITBOX.Mnemonic.generateMnemonic(224, BITBOX.Mnemonic.mnemonicWordLists().spanish);
// broma jabón caballo mostrar joroba cordón serie cumbre infiel dardo nido fijo mafia balanza índice leyenda altivo resina rapaz vereda cómodo

// generate korean mnemonic from 256 bits of entropy
BITBOX.Mnemonic.generateMnemonic(256, BITBOX.Mnemonic.mnemonicWordLists().korean);
// 일기 궁극적 고집 명절 습기 과학 졸음 의논 냉동 모금 남대문 신문 불행 학술 불교 만족 형편 일기 형제 앨범 갑자기 연결 길이 눈썹

// generate japanese mnemonic from 128 bits of entropy
BITBOX.Mnemonic.generateMnemonic(128, BITBOX.Mnemonic.mnemonicWordLists().japanese);
// ひほう　まわり　むぎちゃ　つける　すはだ　かなざわし　ことがら　ふっかつ　くうかん　すいえい　しゃおん　れんさい

// generate a chinese simplified mnemonic from 192 bits of entropy
BITBOX.Mnemonic.generateMnemonic(128, BITBOX.Mnemonic.mnemonicWordLists().chinese_simplified);
// 扭 嘴 挂 乃 赏 限 桂 曰 漏 绅 算 梯

// generate a chinese traditional mnemonic from 160 bits of entropy
BITBOX.Mnemonic.generateMnemonic(128, BITBOX.Mnemonic.mnemonicWordLists().chinese_traditional);
// 騰 編 洛 星 傷 盧 影 訊 寸 士 血 壩
```

[More info on generateMnemonic](https://www.bitbox.earth/bitboxcli/mnemonic#generateMnemonic)

### entropyToMnemonic

In the previous examples we just told BITBOX how many bits of entropy and in what language we wanted our mnemonic and it handled generating the randomness. But what if we already have a source of enropy which we'd like to turn in to a mnemonic. For that there is `entropyToMnemonic`.

For this demonstration we'll use `BITBOX.Crypto.randomBytes` to get our entropy but your entropy could come from another source.

```js
// create 16 bytes of entropy
let entropy1 = BITBOX.Crypto.randomBytes(16);
// 549ef25e358069775e3c4c6ba9652b6f
// create 12 english word mnemonic
BITBOX.Mnemonic.entropyToMnemonic(entropy1)
// fee wasp nurse help alley roof jump maze hill enroll enlist teach

// create 20 bytes of entropy
let entropy2 = BITBOX.Crypto.randomBytes(20);
// 9d115d4d172dca804052a79dc5f18dedf97a3eff
// create chinese traditional mnemonic from 20 bytes of entropy
BITBOX.Mnemonic.entropyToMnemonic(entropy2, BITBOX.Mnemonic.mnemonicWordLists().chinese_traditional)
// 閃 索 衝 研 腔 顯 中 控 牆 織 劉 桃 婚 戰 沫

// create 24 bytes of entropy
let entropy3 = BITBOX.Crypto.randomBytes(24);
// b842e25d09c53d8a469db2a1a6f1a19f58b8c9f9b52feb89
// create 18 french word mnemonic
BITBOX.Mnemonic.entropyToMnemonic(entropy3, BITBOX.Mnemonic.mnemonicWordLists().french)
// phoque aviser matière asticot enfance pyramide bastion pépite myrtille copain flamme débattre lanterne fendoir tailler nomade tiroir école

// create 28 bytes of entropy
let entropy4 = BITBOX.Crypto.randomBytes(28);
// 8162362d2574b8b8fb13de1e4514f0d7873d142fde27af55c5d1e14e
// create korean mnemonic from 28 bytes of entropy
BITBOX.Mnemonic.entropyToMnemonic(entropy4, BITBOX.Mnemonic.mnemonicWordLists().korean)
// 쌍둥이 공원 엉망 방법 방지 빗방울 하늘 식료품 금년 논리 명칭 입학 수화기 언어 제공 주문 자격 입력 순서 조직 통역

// create 32 bytes of entropy
let entropy5 = BITBOX.Crypto.randomBytes(32);
// d2782eaa3041896d57df535c51a2d5e548094ac23b83b6ae221661377bd815b5
// create japanese mnemonic from 32 bytes of entropy
BITBOX.Mnemonic.entropyToMnemonic(entropy5, BITBOX.Mnemonic.mnemonicWordLists().japanese)
// ふうふ　ばかり　ともだち　さんせい　うりあげ　ぬんちゃく　さわやか　むりょう　さつたば　だむる　にんよう　ひこく　そとづら　とうきゅう　いさん　すてる　ぬんちゃく　ずひょう　たいちょう　ばしょ　せぼね　めぐまれる　こんすい　にいがた
```

[More info on entropyToMnemonic](https://www.bitbox.earth/bitboxcli/mnemonic#entropyToMnemonic)

### mnemonicToEntropy

Since mnemonics are created from entropy it make sense that we could turn a mnemonic back to entropy. That's what `mnemonicToEntropy` is for.

```js
let entropy0 = '549ef25e358069775e3c4c6ba9652b6f';
let mnemonic0 = BITBOX.Mnemonic.entropyToMnemonic(entropy0);
BITBOX.Mnemonic.mnemonicToEntropy(mnemonic0)
// 549ef25e358069775e3c4c6ba9652b6f

let entropy1 = '9d115d4d172dca804052a79dc5f18dedf97a3eff';
let mnemonic1 = BITBOX.Mnemonic.entropyToMnemonic(entropy1);
BITBOX.Mnemonic.mnemonicToEntropy(mnemonic1)

let entropy2 = 'b842e25d09c53d8a469db2a1a6f1a19f58b8c9f9b52feb89';
let mnemonic2 = BITBOX.Mnemonic.entropyToMnemonic(entropy2);
BITBOX.Mnemonic.mnemonicToEntropy(mnemonic2)
// b842e25d09c53d8a469db2a1a6f1a19f58b8c9f9b52feb89

let entropy3 = '8162362d2574b8b8fb13de1e4514f0d7873d142fde27af55c5d1e14e';
let mnemonic3 = BITBOX.Mnemonic.entropyToMnemonic(entropy3);
BITBOX.Mnemonic.mnemonicToEntropy(mnemonic3)
// 8162362d2574b8b8fb13de1e4514f0d7873d142fde27af55c5d1e14e

let entropy4 = 'd2782eaa3041896d57df535c51a2d5e548094ac23b83b6ae221661377bd815b5';
let mnemonic4 = BITBOX.Mnemonic.entropyToMnemonic(entropy4);
BITBOX.Mnemonic.mnemonicToEntropy(mnemonic4)
// d2782eaa3041896d57df535c51a2d5e548094ac23b83b6ae221661377bd815b5
```

[More info on entropyToMnemonic](https://www.bitbox.earth/bitboxcli/mnemonic#mnemonicToEntropy)

## validateMnemonic

Now that we are able to generate mnemonics we can validate that they are the correct length and contain words from the original wordlist from which they were generated w/ `validateMnemonic`. It takes as it's 1st argument a mnemonic and as it's 2nd argument the wordlist to validate from.

It will also suggest the closest word in the wordlist if an invalid word is supplied. This works in all 8 supported languages

```js
BITBOX.Mnemonic.validateMnemonic('ea', BITBOX.Mnemonic.mnemonicWordLists().english);
// ea is not in wordlist, did you mean eager?

BITBOX.Mnemonic.validateMnemonic('ふう', BITBOX.Mnemonic.mnemonicWordLists().japanese);
// ふう is not in wordlist, did you mean ふうけい?

BITBOX.Mnemonic.validateMnemonic('ba', BITBOX.Mnemonic.mnemonicWordLists().italian)
// ba is not in wordlist, did you mean babele?

// too long
BITBOX.Mnemonic.validateMnemonic('boil lonely casino manage habit where total glory muffin name limit mansion boil lonely casino manage habit where total glory muffin name limit mansion boil lonely casino manage habit where total glory muffin name limit mansion boil lonely casino manage habit where total glory muffin name limit mansion', BITBOX.Mnemonic.mnemonicWordLists().english);
// Invalid mnemonic

// too short
BITBOX.Mnemonic.validateMnemonic('boil lonely casino', BITBOX.Mnemonic.mnemonicWordLists().english);

// Valid mnemonic
BITBOX.Mnemonic.validateMnemonic('boil lonely casino manage habit where total glory muffin name limit mansion', BITBOX.Mnemonic.mnemonicWordLists().english);
```

[More info on validateMnemonic](https://www.bitbox.earth/bitboxcli/mnemonic#validateMnemonic)

### findNearestWord

If you don't need to fully validate a mnemonic but would like to find the nearest matching word from the original wordlist you can use `findNearestWord`. It takes as arguments a word and a wordlist and returns the nearest matching word. Of course it works for all 8 supported languages.

```js
// english
let word = 'ab';
let wordlist = BITBOX.Mnemonic.mnemonicWordLists().english;
BITBOX.Mnemonic.findNearestWord(word, wordlist);
// abandon

// french
let word = 'octu';
let wordlist = BITBOX.Mnemonic.mnemonicWordLists().french;
BITBOX.Mnemonic.findNearestWord(word, wordlist);
// octupler

// spanish
let word = 'foobaro';
let wordlist = BITBOX.Mnemonic.mnemonicWordLists().spanish;
BITBOX.Mnemonic.findNearestWord(word, wordlist);
// forro

// italian
let word = 'nv';
let wordlist = BITBOX.Mnemonic.mnemonicWordLists().italian;
BITBOX.Mnemonic.findNearestWord(word, wordlist);
// neve
```

[More info on findNearestWord](https://www.bitbox.earth/bitboxcli/mnemonic#findNearestWord)

## mnemonicToSeedHex

Mnemonics can be turned in to [root seeds](https://bitcoin.org/en/developer-guide#hierarchical-deterministic-key-creation) which can then be turned in to HDNodes to generate keypairs and addresses. BITBOX offers two ways to turn mnemonics to seeds. The 1st way is to turn a mnemonic to a seed encoded as hex w/ `mnemonicToSeedHex` which takes as a 1st argument the mnemonic and as an optional 2nd argument a passphrase.

```js
// english mnemonic to root seed encoded as hex
BITBOX.Mnemonic.mnemonicToSeedHex('boil lonely casino manage habit where total glory muffin name limit mansion', '');
// e906236ab5ebec8fbff9948807a6f5d2aa6f35e8bcbcda99e22f9048323cdc0755b781782ee1cce40007bcf900593ed2667e6e9800d734fa46a8f7f51ec74818

// japanese mnemonic to root seed encoded as hex
BITBOX.Mnemonic.mnemonicToSeedHex('먹이 비극 이튿날 여건 타자기 시간 캠페인 여고생 세미나 중단 제한 글자', '');
// ae74b7bc43120fbfb6a76bec970eee94806b81b6ce4ab48be25c2c0e1c9683a36d4dbcdbde30b622a6cb9735f63abc6a7e35334723dba3d8737983235c84df38

// italian mnemonic to root seed encoded as hex
BITBOX.Mnemonic.mnemonicToSeedHex('ginepro erigere sgonfiare mugnaio aria rilassato scatenare derapata avvenire stelo governo sbruffone emanato mini misurare cadetto festivo flamenco', '')
// 389a60b6f2a229f27d609b0d770ada9caf3dfc268e12a4affab386b91f7e59576581ae34ee6d29caf34721b5d44d1b9a3f5b993befa575cbdd62d16b33e5534a

// chinese simplified mnemonic to root seed encoded as hex
BITBOX.Mnemonic.mnemonicToSeedHex('烈 珠 据 牙 蔡 米 津 熔 贺 祖 逃 怎 鲁 穿 胡 听 近 杯', '')
// e1483c898fd53a4db5408e175d42c650d7baa1678ac43ab9f16f3658aea55393fddd7d46c6f8253425b6363fbf54ce9db7720152b57ec686e9695a30ac8d4449

// french mnemonic to root seed encoded as hex
BITBOX.Mnemonic.mnemonicToSeedHex('incarner pulpe libre acclamer vernir vorace obstacle saluer rocheux officier pinceau ossature pivoter ennuyeux fatal sénateur phrase poney', '')
// 0d8a772a9203a1bba96a09490ed2fd1784fcdae7209df2cb1aa31f6aea6f6ff9ac04dd8681f366c49aa96f71e8728952e7c3b56df49d47451bb508dad150d699
```

[More info on mnemonicToSeedHex](https://www.bitbox.earth/bitboxcli/mnemonic#mnemonicToSeedHex)

## mnemonicToSeedBuffer

Another way of turning a mnemonic to a root seed is as a [buffer](https://nodejs.org/api/buffer.html) w/ `mnemonicToSeedBuffer`.

```js
// english mnemonic to root seed buffer
BITBOX.Mnemonic.mnemonicToSeedBuffer('boil lonely casino manage habit where total glory muffin name limit mansion', '');
// <Buffer e9 06 23 6a b5 eb ec 8f bf f9 94 88 07 a6 f5 d2 aa 6f 35 e8 bc bc da 99 e2 2f 90 48 32 3c dc 07 55 b7 81 78 2e e1 cc e4 00 07 bc f9 00 59 3e d2 66 7e ... >

// japanese mnemonic to root seed buffer
BITBOX.Mnemonic.mnemonicToSeedBuffer('먹이 비극 이튿날 여건 타자기 시간 캠페인 여고생 세미나 중단 제한 글자', '');
// <Buffer ae 74 b7 bc 43 12 0f bf b6 a7 6b ec 97 0e ee 94 80 6b 81 b6 ce 4a b4 8b e2 5c 2c 0e 1c 96 83 a3 6d 4d bc db de 30 b6 22 a6 cb 97 35 f6 3a bc 6a 7e 35 ... >

// italian mnemonic to root seed buffer
BITBOX.Mnemonic.mnemonicToSeedBuffer('ginepro erigere sgonfiare mugnaio aria rilassato scatenare derapata avvenire stelo governo sbruffone emanato mini misurare cadetto festivo flamenco', '')
// <Buffer 38 9a 60 b6 f2 a2 29 f2 7d 60 9b 0d 77 0a da 9c af 3d fc 26 8e 12 a4 af fa b3 86 b9 1f 7e 59 57 65 81 ae 34 ee 6d 29 ca f3 47 21 b5 d4 4d 1b 9a 3f 5b ... >

// chinese simplified mnemonic to root seed buffer
BITBOX.Mnemonic.mnemonicToSeedBuffer('烈 珠 据 牙 蔡 米 津 熔 贺 祖 逃 怎 鲁 穿 胡 听 近 杯', '')
// <Buffer e1 48 3c 89 8f d5 3a 4d b5 40 8e 17 5d 42 c6 50 d7 ba a1 67 8a c4 3a b9 f1 6f 36 58 ae a5 53 93 fd dd 7d 46 c6 f8 25 34 25 b6 36 3f bf 54 ce 9d b7 72 ... >

// french mnemonic to root seed buffer
BITBOX.Mnemonic.mnemonicToSeedBuffer('incarner pulpe libre acclamer vernir vorace obstacle saluer rocheux officier pinceau ossature pivoter ennuyeux fatal sénateur phrase poney', '')
// <Buffer 0d 8a 77 2a 92 03 a1 bb a9 6a 09 49 0e d2 fd 17 84 fc da e7 20 9d f2 cb 1a a3 1f 6a ea 6f 6f f9 ac 04 dd 86 81 f3 66 c4 9a a9 6f 71 e8 72 89 52 e7 c3 ... >
```

[More info on mnemonicToSeedBuffer](https://www.bitbox.earth/bitboxcli/mnemonic#mnemonicToSeedBuffer)

## keypairsFromMnemonic

BITBOX offers a helper method for quickly generating keypairs from a mnemonic. It generates the addresses as the `n`th external change address of the 1st account from that mnemonic w/ this derivation path: `m/44’/145’/0’/0/n`.

```js
// 1st create a mnemonic from 32 bytes of random entropy
let mnemonic = BITBOX.Mnemonic.entropyToMnemonic(32);
// symptom owner ridge follow buffalo choose stem depend million jar lemon claw color credit remove model pudding slot fiber west heavy ranch bird wet

// Then call keypairsFromMnemonic and pass in your mnemonic and how many keypairs you'd like
BITBOX.Mnemonic.keypairsFromMnemonic(mnemonic, 5)
// [ { privateKeyWIF: 'Kz6b1TszeUGaypUpRCnfD2L17bQSW93o4j3VMpvT5e5BqaF9XkyP',
// address: 'bitcoincash:qp8a4vzfk9kstwsl4ud4ym3z2tckdf7a4gfwkxvtfq' },
// { privateKeyWIF: 'L5ZHQ2BdTQaTq2A8HNsdkHYKPLsfrHgvJyrVxHFFZyN9K3fmeoiG',
// address: 'bitcoincash:qq5nxh27up6hcm0nn36lxtu7n8a7l6jsj52s8dvtex' },
// { privateKeyWIF: 'KwyY3Z7STwbxnmQXe1vVmXhT8Y3W1BJQpRgteRhTWCyvvdro2j33',
// address: 'bitcoincash:qzj9n9jmnmyeqfdc5k65kxta3c7ch0g3wudeyjeg3y' },
// { privateKeyWIF: 'KxMG2mjL8DZQCaoXz8aFw5XYqguKiDHBb16JwDQMGa7ga7kfy9cE',
// address: 'bitcoincash:qrhj0lesz6sn7l4hc5arh5tt8k583ahdaun6mcdjx8' },
// { privateKeyWIF: 'Kz3qqJ8GFSSbDrBqtV7mfhBoDPkSmMKtp7Yk62psDgmRjyU8id8J',
// address: 'bitcoincash:qp8xjllc75c2hgrpjy3f6kegtfqgmn72dqs0y20anv' } ]

// again in w/ a japanese mnemonic
let mnemonic = BITBOX.Mnemonic.entropyToMnemonic(32, BITBOX.Mnemonic.mnemonicWordLists().japanese);
// ふおん　ふっき　たぶん　めいうん　ゆうめい　とうし　じゅしん　おっと　むのう　はぶらし　りけん　ふきん　じゅんばん　たとえる　こよい　けんお　りせい　らくご　いっせい　いせき　ひいき　じてん　ひてい　むぎちゃ

// [ { privateKeyWIF: 'L17qQgmbX9YgSMPbJKiiWQym2LKwf76fKNeCJnRdp6zQjJNK3MY1',
//     address: 'bitcoincash:qpzf48wla4s3u9nwtpsgkuf3cmwp8a7zasnt6f6w8l' },
//   { privateKeyWIF: 'Kwr65M1aabJf4fY4arALUySgmLpykNmJpLsi6hNPH6qnBXdZ1Xtf',
//     address: 'bitcoincash:qpzj6hcs925umt834hkekesr833l0496wv55awyumj' },
//   { privateKeyWIF: 'L5N5c8Gg6xNMo4KNFjpXiXCJs4eAKDC5etQ4Xtz1fsmKQ4s7h4NU',
//     address: 'bitcoincash:qz42r45avxy9y7pxjqefw5kah9mm3klfecln36d3t7' },
//   { privateKeyWIF: 'L2yDWTnCpZ66m5HQe3jUG3i2VT8xQYt9HgLNYH2CzPGw2U4c8obV',
//     address: 'bitcoincash:qpy8d8cjcp8kmssxn2kldet39ga6xw7smcxwqzsugw' },
//   { privateKeyWIF: 'L2K63GoJcwBCBPKQr1TCDdP44JhFHDVmYJx6CqkjEBq4QAh35JXs',
//     address: 'bitcoincash:qpkrgvqqc6wx90mva4mr76nskau0ydqqkqvrxtvxhl' } ]
```

[More info on keypairsFromMnemonic](https://www.bitbox.earth/bitboxcli/mnemonic#keypairsFromMnemonic)


## Summary

Mnemonics generated from strong entropy are the most secure way of generating Bitcoin Cash root seeds, HD Nodes and keypairs. BITBOX's `Mnemonic` class gives you the tools you need to generate and validate mnemonics in 8 languages.