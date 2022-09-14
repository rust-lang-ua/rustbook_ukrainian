<!-- Old heading. Do not remove or links may break. -->
<a id="closures-anonymous-functions-that-can-capture-their-environment"></a>

## crwdns68086:0crwdne68086:0

crwdns68088:0crwdne68088:0 crwdns68090:0crwdne68090:0 crwdns68092:0crwdne68092:0 crwdns68094:0crwdne68094:0

<!-- Old headings. Do not remove or links may break. -->
<a id="creating-an-abstraction-of-behavior-with-closures"></a>
<a id="refactoring-using-functions"></a>
<a id="refactoring-with-closures-to-store-code"></a>

### crwdns68096:0crwdne68096:0

crwdns68098:0crwdne68098:0 crwdns68100:0crwdne68100:0 crwdns68102:0crwdne68102:0 crwdns68104:0crwdne68104:0 crwdns68106:0crwdne68106:0

crwdns68108:0crwdne68108:0 crwdns68110:0crwdne68110:0 crwdns68112:0crwdne68112:0 crwdns68114:0crwdne68114:0 crwdns68116:0crwdne68116:0

<span class="filename">crwdns68118:0crwdne68118:0</span>

```rust,noplayground
crwdns68120:0crwdne68120:0
```

<span class="caption">crwdns68122:0crwdne68122:0</span>

crwdns68124:0crwdne68124:0 crwdns68126:0crwdne68126:0

crwdns68128:0crwdne68128:0 crwdns68130:0crwdne68130:0 crwdns68132:0crwdne68132:0<!-- ignore --> crwdns68134:0crwdne68134:0 crwdns68136:0crwdne68136:0 crwdns68138:0crwdne68138:0 crwdns68140:0crwdne68140:0

crwdns68142:0crwdne68142:0 crwdns68144:0crwdne68144:0 crwdns68146:0crwdne68146:0 crwdns68148:0crwdne68148:0

crwdns68150:0crwdne68150:0

```console
crwdns68152:0crwdne68152:0
```

crwdns68154:0crwdne68154:0 crwdns68156:0crwdne68156:0 crwdns68158:0crwdne68158:0 crwdns68160:0crwdne68160:0

### crwdns68162:0crwdne68162:0

crwdns68164:0crwdne68164:0 crwdns68166:0crwdne68166:0 crwdns68168:0crwdne68168:0 crwdns68170:0crwdne68170:0 crwdns68172:0crwdne68172:0

crwdns68174:0crwdne68174:0 crwdns68176:0crwdne68176:0

crwdns68178:0crwdne68178:0 crwdns68180:0crwdne68180:0 crwdns68182:0crwdne68182:0

<span class="filename">crwdns68184:0crwdne68184:0</span>

```rust
crwdns68186:0crwdne68186:0
```


<span class="caption">crwdns68188:0crwdne68188:0</span>

crwdns68190:0crwdne68190:0 crwdns68192:0crwdne68192:0 crwdns68194:0crwdne68194:0 crwdns68196:0crwdne68196:0

```rust,ignore
crwdns68198:0crwdne68198:0
```

crwdns68200:0crwdne68200:0 crwdns68202:0crwdne68202:0 crwdns68204:0crwdne68204:0 crwdns68206:0crwdne68206:0 crwdns68208:0crwdne68208:0 crwdns68210:0crwdne68210:0

crwdns68212:0crwdne68212:0 crwdns68214:0crwdne68214:0 crwdns68216:0crwdne68216:0 crwdns68218:0crwdne68218:0 crwdns68220:0crwdne68220:0 crwdns68222:0crwdne68222:0

<span class="filename">crwdns68224:0crwdne68224:0</span>

```rust,ignore,does_not_compile
crwdns68226:0crwdne68226:0
```


<span class="caption">crwdns68228:0crwdne68228:0</span>

crwdns68230:0crwdne68230:0

```console
crwdns68232:0crwdne68232:0
```

crwdns68234:0crwdne68234:0 crwdns68236:0crwdne68236:0

### crwdns68238:0crwdne68238:0

crwdns68240:0crwdne68240:0 crwdns68242:0crwdne68242:0

crwdns68244:0crwdne68244:0

<span class="filename">crwdns68246:0crwdne68246:0</span>

```rust
crwdns68248:0crwdne68248:0
```


<span class="caption">crwdns68250:0crwdne68250:0</span>

crwdns68252:0crwdne68252:0

crwdns68254:0crwdne68254:0 crwdns68256:0crwdne68256:0

```console
crwdns68258:0crwdne68258:0
```

crwdns68260:0crwdne68260:0 crwdns68262:0crwdne68262:0

<span class="filename">crwdns68264:0crwdne68264:0</span>

```rust
crwdns68266:0crwdne68266:0
```


<span class="caption">crwdns68268:0crwdne68268:0</span>

crwdns68270:0crwdne68270:0

```console
crwdns68272:0crwdne68272:0
```

crwdns68274:0crwdne68274:0 crwdns68276:0crwdne68276:0 crwdns68278:0crwdne68278:0 crwdns68280:0crwdne68280:0

crwdns68282:0crwdne68282:0 crwdns68284:0crwdne68284:0 crwdns68286:0crwdne68286:0

<!-- Old headings. Do not remove or links may break. -->
<a id="storing-closures-using-generic-parameters-and-the-fn-traits"></a>
<a id="limitations-of-the-cacher-implementation"></a>
<a id="moving-captured-values-out-of-the-closure-and-the-fn-traits"></a>

### crwdns68288:0crwdne68288:0

crwdns68290:0crwdne68290:0 crwdns68292:0crwdne68292:0

crwdns68294:0crwdne68294:0 crwdns68296:0crwdne68296:0

1. crwdns68298:0crwdne68298:0 crwdns68300:0crwdne68300:0 crwdns68302:0crwdne68302:0
2. crwdns68304:0crwdne68304:0 crwdns68306:0crwdne68306:0
3. crwdns68308:0crwdne68308:0 crwdns68310:0crwdne68310:0

crwdns68312:0crwdne68312:0

```rust,ignore
crwdns68314:0crwdne68314:0
```

crwdns68316:0crwdne68316:0 crwdns68318:0crwdne68318:0

crwdns68320:0crwdne68320:0 crwdns68322:0crwdne68322:0

crwdns68324:0crwdne68324:0 crwdns68326:0crwdne68326:0 crwdns68328:0crwdne68328:0 crwdns68330:0crwdne68330:0 crwdns68332:0crwdne68332:0

> crwdns68334:0crwdne68334:0 crwdns68336:0crwdne68336:0 crwdns68338:0crwdne68338:0

crwdns68340:0crwdne68340:0

crwdns68342:0crwdne68342:0 crwdns68344:0crwdne68344:0 crwdns68346:0crwdne68346:0

<span class="filename">crwdns68348:0crwdne68348:0</span>

```rust
crwdns68350:0crwdne68350:0
```


<span class="caption">crwdns68352:0crwdne68352:0</span>

crwdns68354:0crwdne68354:0

```console
crwdns68356:0crwdne68356:0
```

crwdns68358:0crwdne68358:0 crwdns68360:0crwdne68360:0

crwdns68362:0crwdne68362:0 crwdns68364:0crwdne68364:0

<span class="filename">crwdns68366:0crwdne68366:0</span>

```rust,ignore,does_not_compile
crwdns68368:0crwdne68368:0
```


<span class="caption">crwdns68370:0crwdne68370:0</span>

crwdns68372:0crwdne68372:0 crwdns68374:0crwdne68374:0 crwdns68376:0crwdne68376:0 crwdns68378:0crwdne68378:0 crwdns68380:0crwdne68380:0 crwdns68382:0crwdne68382:0

```console
crwdns68384:0crwdne68384:0
```

crwdns68386:0crwdne68386:0 crwdns68388:0crwdne68388:0 crwdns68390:0crwdne68390:0 crwdns68392:0crwdne68392:0

<span class="filename">crwdns68394:0crwdne68394:0</span>

```rust
crwdns68396:0crwdne68396:0
```


<span class="caption">crwdns68398:0crwdne68398:0</span>

crwdns68400:0crwdne68400:0 crwdns68402:0crwdne68402:0 crwdns68404:0crwdne68404:0
