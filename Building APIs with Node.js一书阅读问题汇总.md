---
title: Building APIs with Node.js一书阅读问题汇总  
date: 2017-01-06 16:16:29  
description: Building APIs with Node.js 遇到问题汇总
---

该书地址：[这里](http://www.apress.com/cn/book/9781484224410)  
该书代码： [这里](https://github.com/caio-ribeiro-pereira/building-apis-with-nodejs)

## 1. 安装sqlite3时出现卡主不动问题：
```
$ npm i  sqlite3 --save
> sqlite3@3.1.8 install /Users/zyx/code/lean-api/ntask-api/node_modules/sqlite3
> node-pre-gyp install --fallback-to-build
```

一直卡到这里不动，而加载其他npm包则没有问题。在查找错误原因时发现了 [http://support.ghost.org/troubleshooting/#sqlite3-errors](http://support.ghost.org/troubleshooting/#sqlite3-errors)
里面有提到通过
>To get more information about what’s failing, run `npm install sqlite3 --loglevel=info`. You should then find a more detailed messages explaining   

通过这个终于发现应该还是 GFW 的原因。
详细信息如下：
```
> sqlite3@3.1.8 install /Users/zyx/code/lean-api/ntask-api/node_modules/sqlite3
> node-pre-gyp install --fallback-to-build

node-pre-gyp info it worked if it ends with ok
node-pre-gyp info using node-pre-gyp@0.6.31
node-pre-gyp info using node@7.3.0 | darwin | x64
node-pre-gyp info check checked for "/Users/zyx/code/lean-api/ntask-api/node_modules/sqlite3/lib/binding/node-v51-darwin-x64/node_sqlite3.node" (not found)
node-pre-gyp http GET https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v3.1.8/node-v51-darwin-x64.tar.gz
node-pre-gyp http 200 https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v3.1.8/node-v51-darwin-x64.tar.gz
node-pre-gyp info install unpacking node_sqlite3.node
node-pre-gyp info tarball done parsing tarball
node-pre-gyp info validate Running test command: '/usr/local/bin/node --eval 'require(\'/Users/zyx/code/lean-api/ntask-api/node_modules/sqlite3/lib/binding/node-v51-darwin-x64/node_sqlite3.node\')''
[sqlite3] Success: "/Users/zyx/code/lean-api/ntask-api/node_modules/sqlite3/lib/binding/node-v51-darwin-x64/node_sqlite3.node" is installed via remote
node-pre-gyp info ok
npm info lifecycle minimist@0.0.8~postinstall: minimist@0.0.8
npm info lifecycle mkdirp@0.5.1~postinstall: mkdirp@0.5.1
npm info lifecycle abbrev@1.0.9~postinstall: abbrev@1.0.9
npm info lifecycle nopt@3.0.6~postinstall: nopt@3.0.6
npm info lifecycle delegates@1.0.0~postinstall: delegates@1.0.0
npm info lifecycle buffer-shims@1.0.0~postinstall: buffer-shims@1.0.0
npm info lifecycle core-util-is@1.0.2~postinstall: core-util-is@1.0.2
npm info lifecycle inherits@2.0.3~postinstall: inherits@2.0.3
npm info lifecycle isarray@1.0.0~postinstall: isarray@1.0.0
npm info lifecycle process-nextick-args@1.0.7~postinstall: process-nextick-args@1.0.7
npm info lifecycle string_decoder@0.10.31~postinstall: string_decoder@0.10.31
npm info lifecycle util-deprecate@1.0.2~postinstall: util-deprecate@1.0.2
npm info lifecycle readable-stream@2.1.5~postinstall: readable-stream@2.1.5
npm info lifecycle are-we-there-yet@1.1.2~postinstall: are-we-there-yet@1.1.2
npm info lifecycle console-control-strings@1.1.0~postinstall: console-control-strings@1.1.0
npm info lifecycle aproba@1.0.4~postinstall: aproba@1.0.4
npm info lifecycle has-color@0.1.7~postinstall: has-color@0.1.7
npm info lifecycle has-unicode@2.0.1~postinstall: has-unicode@2.0.1
npm info lifecycle object-assign@4.1.0~postinstall: object-assign@4.1.0
npm info lifecycle signal-exit@3.0.1~postinstall: signal-exit@3.0.1
npm info lifecycle number-is-nan@1.0.1~postinstall: number-is-nan@1.0.1
npm info lifecycle code-point-at@1.0.1~postinstall: code-point-at@1.0.1
npm info lifecycle number-is-nan@1.0.1~postinstall: number-is-nan@1.0.1
npm info lifecycle is-fullwidth-code-point@1.0.0~postinstall: is-fullwidth-code-point@1.0.0
npm info lifecycle ansi-regex@2.0.0~postinstall: ansi-regex@2.0.0
npm info lifecycle strip-ansi@3.0.1~postinstall: strip-ansi@3.0.1
npm info lifecycle string-width@1.0.2~postinstall: string-width@1.0.2
npm info lifecycle wide-align@1.1.0~postinstall: wide-align@1.1.0
npm info lifecycle gauge@2.6.0~postinstall: gauge@2.6.0
npm info lifecycle set-blocking@2.0.0~postinstall: set-blocking@2.0.0
npm info lifecycle npmlog@4.0.0~postinstall: npmlog@4.0.0
npm info lifecycle deep-extend@0.4.1~postinstall: deep-extend@0.4.1
npm info lifecycle ini@1.3.4~postinstall: ini@1.3.4
npm info lifecycle minimist@1.2.0~postinstall: minimist@1.2.0
npm info lifecycle strip-json-comments@1.0.4~postinstall: strip-json-comments@1.0.4
npm info lifecycle rc@1.1.6~postinstall: rc@1.1.6
npm info lifecycle aws-sign2@0.6.0~postinstall: aws-sign2@0.6.0
npm info lifecycle aws4@1.5.0~postinstall: aws4@1.5.0
npm info lifecycle caseless@0.11.0~postinstall: caseless@0.11.0
npm info lifecycle delayed-stream@1.0.0~postinstall: delayed-stream@1.0.0
npm info lifecycle combined-stream@1.0.5~postinstall: combined-stream@1.0.5
npm info lifecycle extend@3.0.0~postinstall: extend@3.0.0
npm info lifecycle forever-agent@0.6.1~postinstall: forever-agent@0.6.1
npm info lifecycle asynckit@0.4.0~postinstall: asynckit@0.4.0
npm info lifecycle ansi-styles@2.2.1~postinstall: ansi-styles@2.2.1
npm info lifecycle escape-string-regexp@1.0.5~postinstall: escape-string-regexp@1.0.5
npm info lifecycle ansi-regex@2.0.0~postinstall: ansi-regex@2.0.0
npm info lifecycle has-ansi@2.0.0~postinstall: has-ansi@2.0.0
npm info lifecycle ansi-regex@2.0.0~postinstall: ansi-regex@2.0.0
npm info lifecycle strip-ansi@3.0.1~postinstall: strip-ansi@3.0.1
npm info lifecycle supports-color@2.0.0~postinstall: supports-color@2.0.0
npm info lifecycle chalk@1.1.3~postinstall: chalk@1.1.3
npm info lifecycle graceful-readlink@1.0.1~postinstall: graceful-readlink@1.0.1
npm info lifecycle commander@2.9.0~postinstall: commander@2.9.0
npm info lifecycle generate-function@2.0.0~postinstall: generate-function@2.0.0
npm info lifecycle is-property@1.0.2~postinstall: is-property@1.0.2
npm info lifecycle generate-object-property@1.2.0~postinstall: generate-object-property@1.2.0
npm info lifecycle jsonpointer@4.0.0~postinstall: jsonpointer@4.0.0
npm info lifecycle xtend@4.0.1~postinstall: xtend@4.0.1
npm info lifecycle is-my-json-valid@2.15.0~postinstall: is-my-json-valid@2.15.0
npm info lifecycle pinkie@2.0.4~postinstall: pinkie@2.0.4
npm info lifecycle pinkie-promise@2.0.1~postinstall: pinkie-promise@2.0.1
npm info lifecycle har-validator@2.0.6~postinstall: har-validator@2.0.6
npm info lifecycle hoek@2.16.3~postinstall: hoek@2.16.3
npm info lifecycle boom@2.10.1~postinstall: boom@2.10.1
npm info lifecycle cryptiles@2.0.5~postinstall: cryptiles@2.0.5
npm info lifecycle sntp@1.0.9~postinstall: sntp@1.0.9
npm info lifecycle hawk@3.1.3~postinstall: hawk@3.1.3
npm info lifecycle assert-plus@0.2.0~postinstall: assert-plus@0.2.0
npm info lifecycle extsprintf@1.0.2~postinstall: extsprintf@1.0.2
npm info lifecycle json-schema@0.2.3~postinstall: json-schema@0.2.3
npm info lifecycle verror@1.3.6~postinstall: verror@1.3.6
npm info lifecycle jsprim@1.3.1~postinstall: jsprim@1.3.1
npm info lifecycle asn1@0.2.3~postinstall: asn1@0.2.3
npm info lifecycle assert-plus@1.0.0~postinstall: assert-plus@1.0.0
npm info lifecycle dashdash@1.14.0~postinstall: dashdash@1.14.0
npm info lifecycle getpass@0.1.6~postinstall: getpass@0.1.6
npm info lifecycle jsbn@0.1.0~postinstall: jsbn@0.1.0
npm info lifecycle ecc-jsbn@0.1.1~postinstall: ecc-jsbn@0.1.1
npm info lifecycle jodid25519@1.0.2~postinstall: jodid25519@1.0.2
npm info lifecycle tweetnacl@0.14.3~postinstall: tweetnacl@0.14.3
npm info lifecycle bcrypt-pbkdf@1.0.0~postinstall: bcrypt-pbkdf@1.0.0
npm info lifecycle sshpk@1.10.1~postinstall: sshpk@1.10.1
npm info lifecycle http-signature@1.1.1~postinstall: http-signature@1.1.1
npm info lifecycle is-typedarray@1.0.0~postinstall: is-typedarray@1.0.0
npm info lifecycle isstream@0.1.2~postinstall: isstream@0.1.2
npm info lifecycle json-stringify-safe@5.0.1~postinstall: json-stringify-safe@5.0.1
npm info lifecycle mime-db@1.24.0~postinstall: mime-db@1.24.0
npm info lifecycle mime-types@2.1.12~postinstall: mime-types@2.1.12
npm info lifecycle form-data@2.1.1~postinstall: form-data@2.1.1
npm info lifecycle node-uuid@1.4.7~postinstall: node-uuid@1.4.7
npm info lifecycle oauth-sign@0.8.2~postinstall: oauth-sign@0.8.2
npm info lifecycle qs@6.3.0~postinstall: qs@6.3.0
npm info lifecycle stringstream@0.0.5~postinstall: stringstream@0.0.5
npm info lifecycle punycode@1.4.1~postinstall: punycode@1.4.1
npm info lifecycle tough-cookie@2.3.2~postinstall: tough-cookie@2.3.2
npm info lifecycle tunnel-agent@0.4.3~postinstall: tunnel-agent@0.4.3
npm info lifecycle request@2.76.0~postinstall: request@2.76.0
npm info lifecycle fs.realpath@1.0.0~postinstall: fs.realpath@1.0.0
npm info lifecycle wrappy@1.0.2~postinstall: wrappy@1.0.2
npm info lifecycle inherits@2.0.3~postinstall: inherits@2.0.3
npm info lifecycle balanced-match@0.4.2~postinstall: balanced-match@0.4.2
npm info lifecycle concat-map@0.0.1~postinstall: concat-map@0.0.1
npm info lifecycle brace-expansion@1.1.6~postinstall: brace-expansion@1.1.6
npm info lifecycle minimatch@3.0.3~postinstall: minimatch@3.0.3
npm info lifecycle wrappy@1.0.2~postinstall: wrappy@1.0.2
npm info lifecycle once@1.4.0~postinstall: once@1.4.0
npm info lifecycle inflight@1.0.6~postinstall: inflight@1.0.6
npm info lifecycle path-is-absolute@1.0.1~postinstall: path-is-absolute@1.0.1
npm info lifecycle glob@7.1.1~postinstall: glob@7.1.1
npm info lifecycle rimraf@2.5.4~postinstall: rimraf@2.5.4
npm info lifecycle semver@5.3.0~postinstall: semver@5.3.0
npm info lifecycle ms@0.7.1~postinstall: ms@0.7.1
npm info lifecycle debug@2.2.0~postinstall: debug@2.2.0
npm info lifecycle inherits@2.0.3~postinstall: inherits@2.0.3
npm info lifecycle balanced-match@0.4.2~postinstall: balanced-match@0.4.2
npm info lifecycle concat-map@0.0.1~postinstall: concat-map@0.0.1
npm info lifecycle brace-expansion@1.1.6~postinstall: brace-expansion@1.1.6
npm info lifecycle minimatch@3.0.3~postinstall: minimatch@3.0.3
npm info lifecycle graceful-fs@4.1.9~postinstall: graceful-fs@4.1.9
npm info lifecycle inherits@2.0.3~postinstall: inherits@2.0.3
npm info lifecycle fstream@1.0.10~postinstall: fstream@1.0.10
npm info lifecycle fstream-ignore@1.0.5~postinstall: fstream-ignore@1.0.5
npm info lifecycle wrappy@1.0.2~postinstall: wrappy@1.0.2
npm info lifecycle once@1.3.3~postinstall: once@1.3.3
npm info lifecycle buffer-shims@1.0.0~postinstall: buffer-shims@1.0.0
npm info lifecycle core-util-is@1.0.2~postinstall: core-util-is@1.0.2
npm info lifecycle inherits@2.0.3~postinstall: inherits@2.0.3
npm info lifecycle isarray@1.0.0~postinstall: isarray@1.0.0
npm info lifecycle process-nextick-args@1.0.7~postinstall: process-nextick-args@1.0.7
npm info lifecycle string_decoder@0.10.31~postinstall: string_decoder@0.10.31
npm info lifecycle util-deprecate@1.0.2~postinstall: util-deprecate@1.0.2
npm info lifecycle readable-stream@2.1.5~postinstall: readable-stream@2.1.5
npm info lifecycle uid-number@0.0.6~postinstall: uid-number@0.0.6
npm info lifecycle graceful-fs@4.1.9~postinstall: graceful-fs@4.1.9
npm info lifecycle inherits@2.0.3~postinstall: inherits@2.0.3
npm info lifecycle block-stream@0.0.9~postinstall: block-stream@0.0.9
npm info lifecycle fstream@1.0.10~postinstall: fstream@1.0.10
npm info lifecycle tar@2.2.1~postinstall: tar@2.2.1
npm info lifecycle tar-pack@3.3.0~postinstall: tar-pack@3.3.0
npm info lifecycle nan@2.4.0~postinstall: nan@2.4.0
npm info lifecycle node-pre-gyp@0.6.31~postinstall: node-pre-gyp@0.6.31
npm info lifecycle sqlite3@3.1.8~postinstall: sqlite3@3.1.8
ntask-api@1.0.0 /Users/zyx/code/lean-api/ntask-api
└─┬ sqlite3@3.1.8
  ├── nan@2.4.0
  └─┬ node-pre-gyp@0.6.31
    ├─┬ mkdirp@0.5.1
    │ └── minimist@0.0.8
    ├─┬ nopt@3.0.6
    │ └── abbrev@1.0.9
    ├─┬ npmlog@4.0.0
    │ ├─┬ are-we-there-yet@1.1.2
    │ │ ├── delegates@1.0.0
    │ │ └─┬ readable-stream@2.1.5
    │ │   ├── buffer-shims@1.0.0
    │ │   ├── core-util-is@1.0.2
    │ │   ├── inherits@2.0.3
    │ │   ├── isarray@1.0.0
    │ │   ├── process-nextick-args@1.0.7
    │ │   ├── string_decoder@0.10.31
    │ │   └── util-deprecate@1.0.2
    │ ├── console-control-strings@1.1.0
    │ ├─┬ gauge@2.6.0
    │ │ ├── aproba@1.0.4
    │ │ ├── has-color@0.1.7
    │ │ ├── has-unicode@2.0.1
    │ │ ├── object-assign@4.1.0
    │ │ ├── signal-exit@3.0.1
    │ │ ├─┬ string-width@1.0.2
    │ │ │ ├─┬ code-point-at@1.0.1
    │ │ │ │ └── number-is-nan@1.0.1
    │ │ │ └─┬ is-fullwidth-code-point@1.0.0
    │ │ │   └── number-is-nan@1.0.1
    │ │ ├─┬ strip-ansi@3.0.1
    │ │ │ └── ansi-regex@2.0.0
    │ │ └── wide-align@1.1.0
    │ └── set-blocking@2.0.0
    ├─┬ rc@1.1.6
    │ ├── deep-extend@0.4.1
    │ ├── ini@1.3.4
    │ ├── minimist@1.2.0
    │ └── strip-json-comments@1.0.4
    ├─┬ request@2.76.0
    │ ├── aws-sign2@0.6.0
    │ ├── aws4@1.5.0
    │ ├── caseless@0.11.0
    │ ├─┬ combined-stream@1.0.5
    │ │ └── delayed-stream@1.0.0
    │ ├── extend@3.0.0
    │ ├── forever-agent@0.6.1
    │ ├─┬ form-data@2.1.1
    │ │ └── asynckit@0.4.0
    │ ├─┬ har-validator@2.0.6
    │ │ ├─┬ chalk@1.1.3
    │ │ │ ├── ansi-styles@2.2.1
    │ │ │ ├── escape-string-regexp@1.0.5
    │ │ │ ├─┬ has-ansi@2.0.0
    │ │ │ │ └── ansi-regex@2.0.0
    │ │ │ ├─┬ strip-ansi@3.0.1
    │ │ │ │ └── ansi-regex@2.0.0
    │ │ │ └── supports-color@2.0.0
    │ │ ├─┬ commander@2.9.0
    │ │ │ └── graceful-readlink@1.0.1
    │ │ ├─┬ is-my-json-valid@2.15.0
    │ │ │ ├── generate-function@2.0.0
    │ │ │ ├─┬ generate-object-property@1.2.0
    │ │ │ │ └── is-property@1.0.2
    │ │ │ ├── jsonpointer@4.0.0
    │ │ │ └── xtend@4.0.1
    │ │ └─┬ pinkie-promise@2.0.1
    │ │   └── pinkie@2.0.4
    │ ├─┬ hawk@3.1.3
    │ │ ├── boom@2.10.1
    │ │ ├── cryptiles@2.0.5
    │ │ ├── hoek@2.16.3
    │ │ └── sntp@1.0.9
    │ ├─┬ http-signature@1.1.1
    │ │ ├── assert-plus@0.2.0
    │ │ ├─┬ jsprim@1.3.1
    │ │ │ ├── extsprintf@1.0.2
    │ │ │ ├── json-schema@0.2.3
    │ │ │ └── verror@1.3.6
    │ │ └─┬ sshpk@1.10.1
    │ │   ├── asn1@0.2.3
    │ │   ├── assert-plus@1.0.0
    │ │   ├── bcrypt-pbkdf@1.0.0
    │ │   ├── dashdash@1.14.0
    │ │   ├── ecc-jsbn@0.1.1
    │ │   ├── getpass@0.1.6
    │ │   ├── jodid25519@1.0.2
    │ │   ├── jsbn@0.1.0
    │ │   └── tweetnacl@0.14.3
    │ ├── is-typedarray@1.0.0
    │ ├── isstream@0.1.2
    │ ├── json-stringify-safe@5.0.1
    │ ├─┬ mime-types@2.1.12
    │ │ └── mime-db@1.24.0
    │ ├── node-uuid@1.4.7
    │ ├── oauth-sign@0.8.2
    │ ├── qs@6.3.0
    │ ├── stringstream@0.0.5
    │ ├─┬ tough-cookie@2.3.2
    │ │ └── punycode@1.4.1
    │ └── tunnel-agent@0.4.3
    ├─┬ rimraf@2.5.4
    │ └─┬ glob@7.1.1
    │   ├── fs.realpath@1.0.0
    │   ├─┬ inflight@1.0.6
    │   │ └── wrappy@1.0.2
    │   ├── inherits@2.0.3
    │   ├─┬ minimatch@3.0.3
    │   │ └─┬ brace-expansion@1.1.6
    │   │   ├── balanced-match@0.4.2
    │   │   └── concat-map@0.0.1
    │   ├─┬ once@1.4.0
    │   │ └── wrappy@1.0.2
    │   └── path-is-absolute@1.0.1
    ├── semver@5.3.0
    ├─┬ tar@2.2.1
    │ ├── block-stream@0.0.9
    │ ├─┬ fstream@1.0.10
    │ │ └── graceful-fs@4.1.9
    │ └── inherits@2.0.3
    └─┬ tar-pack@3.3.0
      ├─┬ debug@2.2.0
      │ └── ms@0.7.1
      ├─┬ fstream@1.0.10
      │ ├── graceful-fs@4.1.9
      │ └── inherits@2.0.3
      ├─┬ fstream-ignore@1.0.5
      │ ├── inherits@2.0.3
      │ └─┬ minimatch@3.0.3
      │   └─┬ brace-expansion@1.1.6
      │     ├── balanced-match@0.4.2
      │     └── concat-map@0.0.1
      ├─┬ once@1.3.3
      │ └── wrappy@1.0.2
      ├─┬ readable-stream@2.1.5
      │ ├── buffer-shims@1.0.0
      │ ├── core-util-is@1.0.2
      │ ├── inherits@2.0.3
      │ ├── isarray@1.0.0
      │ ├── process-nextick-args@1.0.7
      │ ├── string_decoder@0.10.31
      │ └── util-deprecate@1.0.2
      └── uid-number@0.0.6

npm WARN ntask-api@1.0.0 No description
npm WARN ntask-api@1.0.0 No repository field.
npm info ok
```


## 2.安装bcrypt包问题 Error: bcrypt@0.8.7 install: `node-gyp rebuild`

```
Agreeing to the Xcode/iOS license requires admin privileges, please re-run as root via sudo.
Agreeing to the Xcode/iOS license requires admin privileges, please re-run as root via sudo.
```

解决方法如下：
>this generally occurs after the mac OS update. All you need to do in terminal is run `sudo xcodebuild -license` and then agree to the terms. and then `npm install` is ok

[key about the solution](http://stackoverflow.com/questions/32720476/agreeing-to-the-xcode-ios-license-requires-admin-privileges-please-re-run-as-ro)
