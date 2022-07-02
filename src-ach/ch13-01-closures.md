<!-- Old heading. Do not remove or links may break. -->
<a id="closures-anonymous-functions-that-can-capture-their-environment"></a>

## crwdns12030:0crwdne12030:0

crwdns12032:0crwdne12032:0 crwdns12034:0crwdne12034:0 crwdns12036:0crwdne12036:0 crwdns12038:0crwdne12038:0

<!-- Old headings. Do not remove or links may break. -->
<a id="creating-an-abstraction-of-behavior-with-closures"></a>
<a id="refactoring-using-functions"></a>
<a id="refactoring-with-closures-to-store-code"></a>

### crwdns12040:0crwdne12040:0

crwdns12042:0crwdne12042:0 crwdns12044:0crwdne12044:0 crwdns12046:0crwdne12046:0 crwdns12048:0crwdne12048:0 crwdns12050:0crwdne12050:0

crwdns12052:0crwdne12052:0 crwdns12054:0crwdne12054:0 crwdns12056:0crwdne12056:0 crwdns12058:0crwdne12058:0 crwdns12060:0crwdne12060:0

<span class="filename">crwdns12062:0crwdne12062:0</span>

```rust,noplayground
crwdns12064:0crwdne12064:0
```

<span class="caption">crwdns12066:0crwdne12066:0</span>

crwdns12068:0crwdne12068:0 crwdns12070:0crwdne12070:0

crwdns12072:0crwdne12072:0 crwdns12074:0crwdne12074:0 crwdns12076:0crwdne12076:0<!-- ignore --> crwdns12078:0crwdne12078:0 crwdns12080:0crwdne12080:0 crwdns12082:0crwdne12082:0 crwdns12084:0crwdne12084:0

crwdns12086:0crwdne12086:0 crwdns12088:0crwdne12088:0 crwdns12090:0crwdne12090:0 crwdns12092:0crwdne12092:0

crwdns12094:0crwdne12094:0

```console
crwdns12096:0crwdne12096:0
```

crwdns12098:0crwdne12098:0 crwdns12100:0crwdne12100:0 crwdns12102:0crwdne12102:0 crwdns12104:0crwdne12104:0

### crwdns12106:0crwdne12106:0

crwdns12108:0crwdne12108:0 crwdns12110:0crwdne12110:0 crwdns12112:0crwdne12112:0 crwdns12114:0crwdne12114:0 crwdns12116:0crwdne12116:0

crwdns12118:0crwdne12118:0 crwdns12120:0crwdne12120:0

crwdns12122:0crwdne12122:0 crwdns12124:0crwdne12124:0 crwdns12126:0crwdne12126:0

<span class="filename">crwdns12128:0crwdne12128:0</span>

```rust
crwdns12130:0crwdne12130:0
```


<span class="caption">crwdns12132:0crwdne12132:0</span>

crwdns12134:0crwdne12134:0 crwdns12136:0crwdne12136:0 crwdns12138:0crwdne12138:0 crwdns12140:0crwdne12140:0

```rust,ignore
crwdns12142:0crwdne12142:0
```

crwdns12144:0crwdne12144:0 crwdns12146:0crwdne12146:0 crwdns12148:0crwdne12148:0 crwdns12150:0crwdne12150:0 crwdns12152:0crwdne12152:0 crwdns12154:0crwdne12154:0

crwdns12156:0crwdne12156:0 crwdns12158:0crwdne12158:0 crwdns12160:0crwdne12160:0 crwdns12162:0crwdne12162:0 crwdns12164:0crwdne12164:0 crwdns12166:0crwdne12166:0

<span class="filename">crwdns12168:0crwdne12168:0</span>

```rust,ignore,does_not_compile
crwdns12170:0crwdne12170:0
```


<span class="caption">crwdns12172:0crwdne12172:0</span>

crwdns12174:0crwdne12174:0

```console
crwdns12176:0crwdne12176:0
```

crwdns12178:0crwdne12178:0 crwdns12180:0crwdne12180:0

### crwdns12182:0crwdne12182:0

crwdns12184:0crwdne12184:0 crwdns12186:0crwdne12186:0

crwdns12188:0crwdne12188:0

<span class="filename">crwdns12190:0crwdne12190:0</span>

```rust
crwdns12192:0crwdne12192:0
```


<span class="caption">crwdns12194:0crwdne12194:0</span>

crwdns12196:0crwdne12196:0

crwdns12198:0crwdne12198:0 crwdns12200:0crwdne12200:0

```console
crwdns12202:0crwdne12202:0
```

crwdns12204:0crwdne12204:0 crwdns12206:0crwdne12206:0

<span class="filename">crwdns12208:0crwdne12208:0</span>

```rust
crwdns12210:0crwdne12210:0
```


<span class="caption">crwdns12212:0crwdne12212:0</span>

crwdns12214:0crwdne12214:0

```console
crwdns12216:0crwdne12216:0
```

crwdns12218:0crwdne12218:0 crwdns12220:0crwdne12220:0 crwdns12222:0crwdne12222:0 crwdns12224:0crwdne12224:0

crwdns12226:0crwdne12226:0 crwdns12228:0crwdne12228:0 crwdns12230:0crwdne12230:0

<!-- Old headings. Do not remove or links may break. -->
<a id="storing-closures-using-generic-parameters-and-the-fn-traits"></a>
<a id="limitations-of-the-cacher-implementation"></a>
<a id="moving-captured-values-out-of-the-closure-and-the-fn-traits"></a>

### crwdns12232:0crwdne12232:0

crwdns12234:0crwdne12234:0 crwdns12236:0crwdne12236:0

crwdns12238:0crwdne12238:0 crwdns12240:0crwdne12240:0

1. crwdns12242:0crwdne12242:0 crwdns12244:0crwdne12244:0 crwdns12246:0crwdne12246:0
2. crwdns12248:0crwdne12248:0 crwdns12250:0crwdne12250:0
3. crwdns12252:0crwdne12252:0 crwdns12254:0crwdne12254:0

crwdns12256:0crwdne12256:0

```rust,ignore
crwdns12258:0crwdne12258:0
```

crwdns12260:0crwdne12260:0 crwdns12262:0crwdne12262:0

crwdns12264:0crwdne12264:0 crwdns12266:0crwdne12266:0

crwdns12268:0crwdne12268:0 crwdns12270:0crwdne12270:0 crwdns12272:0crwdne12272:0 crwdns12274:0crwdne12274:0 crwdns12276:0crwdne12276:0

> crwdns12278:0crwdne12278:0 crwdns12280:0crwdne12280:0 crwdns12282:0crwdne12282:0

crwdns12284:0crwdne12284:0

crwdns12286:0crwdne12286:0 crwdns12288:0crwdne12288:0 crwdns12290:0crwdne12290:0

<span class="filename">crwdns12292:0crwdne12292:0</span>

```rust
crwdns12294:0crwdne12294:0
```


<span class="caption">crwdns12296:0crwdne12296:0</span>

crwdns12298:0crwdne12298:0

```console
crwdns12300:0crwdne12300:0
```

crwdns12302:0crwdne12302:0 crwdns12304:0crwdne12304:0

crwdns12306:0crwdne12306:0 crwdns12308:0crwdne12308:0

<span class="filename">crwdns12310:0crwdne12310:0</span>

```rust,ignore,does_not_compile
crwdns12312:0crwdne12312:0
```


<span class="caption">crwdns12314:0crwdne12314:0</span>

crwdns12316:0crwdne12316:0 crwdns12318:0crwdne12318:0 crwdns12320:0crwdne12320:0 crwdns12322:0crwdne12322:0 crwdns12324:0crwdne12324:0 crwdns12326:0crwdne12326:0

```console
crwdns12328:0crwdne12328:0
```

crwdns12330:0crwdne12330:0 crwdns12332:0crwdne12332:0 crwdns12334:0crwdne12334:0 crwdns12336:0crwdne12336:0

<span class="filename">crwdns12338:0crwdne12338:0</span>

```rust
crwdns12340:0crwdne12340:0
```


<span class="caption">crwdns12342:0crwdne12342:0</span>

crwdns12344:0crwdne12344:0 crwdns12346:0crwdne12346:0 crwdns12348:0crwdne12348:0
