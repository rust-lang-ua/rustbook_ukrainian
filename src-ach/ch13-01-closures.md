<!-- Old heading. Do not remove or links may break. -->
<a id="closures-anonymous-functions-that-can-capture-their-environment"></a>

## crwdns36196:0crwdne36196:0

crwdns36198:0crwdne36198:0 crwdns36200:0crwdne36200:0 crwdns36202:0crwdne36202:0 crwdns36204:0crwdne36204:0

<!-- Old headings. Do not remove or links may break. -->
<a id="creating-an-abstraction-of-behavior-with-closures"></a>
<a id="refactoring-using-functions"></a>
<a id="refactoring-with-closures-to-store-code"></a>

### crwdns36206:0crwdne36206:0

crwdns36208:0crwdne36208:0 crwdns36210:0crwdne36210:0 crwdns36212:0crwdne36212:0 crwdns36214:0crwdne36214:0 crwdns36216:0crwdne36216:0

crwdns36218:0crwdne36218:0 crwdns36220:0crwdne36220:0 crwdns36222:0crwdne36222:0 crwdns36224:0crwdne36224:0 crwdns36226:0crwdne36226:0

<span class="filename">crwdns36228:0crwdne36228:0</span>

```rust,noplayground
crwdns36230:0crwdne36230:0
```

<span class="caption">crwdns36232:0crwdne36232:0</span>

crwdns36234:0crwdne36234:0 crwdns36236:0crwdne36236:0

crwdns36238:0crwdne36238:0 crwdns36240:0crwdne36240:0 crwdns36242:0crwdne36242:0<!-- ignore --> crwdns36244:0crwdne36244:0 crwdns36246:0crwdne36246:0 crwdns36248:0crwdne36248:0 crwdns36250:0crwdne36250:0

crwdns36252:0crwdne36252:0 crwdns36254:0crwdne36254:0 crwdns36256:0crwdne36256:0 crwdns36258:0crwdne36258:0

crwdns36260:0crwdne36260:0

```console
crwdns36262:0crwdne36262:0
```

crwdns36266:0crwdne36266:0 crwdns36270:0crwdne36270:0 crwdns36272:0crwdne36272:0 crwdns36274:0crwdne36274:0

### crwdns36278:0crwdne36278:0

crwdns36282:0crwdne36282:0 crwdns36284:0crwdne36284:0 crwdns36286:0crwdne36286:0 crwdns36294:0crwdne36294:0 crwdns36298:0crwdne36298:0

crwdns36302:0crwdne36302:0 crwdns36304:0crwdne36304:0

crwdns36312:0crwdne36312:0 crwdns36314:0crwdne36314:0 crwdns36316:0crwdne36316:0

<span class="filename">crwdns36320:0crwdne36320:0</span>

```rust
crwdns36322:0crwdne36322:0
```


<span class="caption">crwdns36324:0crwdne36324:0</span>

crwdns36330:0crwdne36330:0 crwdns36334:0crwdne36334:0 crwdns36336:0crwdne36336:0 crwdns36340:0crwdne36340:0

```rust,ignore
crwdns36342:0crwdne36342:0
```

crwdns36346:0crwdne36346:0 crwdns36350:0crwdne36350:0 crwdns36356:0crwdne36356:0 crwdns36358:0crwdne36358:0 crwdns36362:0crwdne36362:0 crwdns36366:0crwdne36366:0

crwdns36370:0crwdne36370:0 crwdns36374:0crwdne36374:0 crwdns36378:0crwdne36378:0 crwdns36382:0crwdne36382:0 crwdns36388:0crwdne36388:0 crwdns36394:0crwdne36394:0

<span class="filename">crwdns36398:0crwdne36398:0</span>

```rust,ignore,does_not_compile
crwdns36400:0crwdne36400:0
```


<span class="caption">crwdns36404:0crwdne36404:0</span>

crwdns36408:0crwdne36408:0

```console
crwdns36410:0crwdne36410:0
```

crwdns36414:0crwdne36414:0 crwdns36418:0crwdne36418:0

### crwdns36422:0crwdne36422:0

crwdns36426:0crwdne36426:0 crwdns36430:0crwdne36430:0

crwdns36432:0crwdne36432:0

<span class="filename">crwdns36442:0crwdne36442:0</span>

```rust
crwdns36444:0crwdne36444:0
```


<span class="caption">crwdns36448:0crwdne36448:0</span>

crwdns36452:0crwdne36452:0

crwdns36454:0crwdne36454:0 crwdns36456:0crwdne36456:0

```console
crwdns36460:0crwdne36460:0
```

crwdns36466:0crwdne36466:0 crwdns36470:0crwdne36470:0

<span class="filename">crwdns36474:0crwdne36474:0</span>

```rust
crwdns36478:0crwdne36478:0
```


<span class="caption">crwdns36482:0crwdne36482:0</span>

crwdns36486:0crwdne36486:0

```console
crwdns36488:0crwdne36488:0
```

crwdns36492:0crwdne36492:0 crwdns36494:0crwdne36494:0 crwdns36498:0crwdne36498:0 crwdns36500:0crwdne36500:0

crwdns36508:0crwdne36508:0 crwdns36512:0crwdne36512:0 crwdns36514:0crwdne36514:0

<!-- Old headings. Do not remove or links may break. -->
<a id="storing-closures-using-generic-parameters-and-the-fn-traits"></a>
<a id="limitations-of-the-cacher-implementation"></a>
<a id="moving-captured-values-out-of-the-closure-and-the-fn-traits"></a>

### crwdns36518:0crwdne36518:0

crwdns36522:0crwdne36522:0 crwdns36526:0crwdne36526:0

crwdns36528:0crwdne36528:0 crwdns36532:0crwdne36532:0

1. crwdns36536:0crwdne36536:0 crwdns36538:0crwdne36538:0 crwdns36542:0crwdne36542:0
2. crwdns36546:0crwdne36546:0 crwdns36550:0crwdne36550:0
3. crwdns36552:0crwdne36552:0 crwdns36554:0crwdne36554:0

crwdns36556:0crwdne36556:0

```rust,ignore
crwdns36558:0crwdne36558:0
```

crwdns36560:0crwdne36560:0 crwdns36562:0crwdne36562:0

crwdns36564:0crwdne36564:0 crwdns36566:0crwdne36566:0

crwdns36568:0crwdne36568:0 crwdns36570:0crwdne36570:0 crwdns36572:0crwdne36572:0 crwdns36574:0crwdne36574:0 crwdns36576:0crwdne36576:0

> crwdns36578:0crwdne36578:0 crwdns36580:0crwdne36580:0 crwdns36582:0crwdne36582:0

crwdns36584:0crwdne36584:0

crwdns36586:0crwdne36586:0 crwdns36588:0crwdne36588:0 crwdns36590:0crwdne36590:0

<span class="filename">crwdns36592:0crwdne36592:0</span>

```rust
crwdns36594:0crwdne36594:0
```


<span class="caption">crwdns36596:0crwdne36596:0</span>

crwdns36598:0crwdne36598:0

```console
crwdns36600:0crwdne36600:0
```

crwdns36602:0crwdne36602:0 crwdns36604:0crwdne36604:0

crwdns36606:0crwdne36606:0 crwdns36608:0crwdne36608:0

<span class="filename">crwdns36610:0crwdne36610:0</span>

```rust,ignore,does_not_compile
crwdns36612:0crwdne36612:0
```


<span class="caption">crwdns36614:0crwdne36614:0</span>

crwdns36616:0crwdne36616:0 crwdns36618:0crwdne36618:0 crwdns36620:0crwdne36620:0 crwdns36622:0crwdne36622:0 crwdns36624:0crwdne36624:0 crwdns36626:0crwdne36626:0

```console
crwdns36628:0crwdne36628:0
```

crwdns36630:0crwdne36630:0 crwdns36632:0crwdne36632:0 crwdns36634:0crwdne36634:0 crwdns36636:0crwdne36636:0

<span class="filename">crwdns36638:0crwdne36638:0</span>

```rust
crwdns36640:0crwdne36640:0
```


<span class="caption">crwdns36642:0crwdne36642:0</span>

crwdns36644:0crwdne36644:0 crwdns36646:0crwdne36646:0 crwdns36648:0crwdne36648:0
