## crwdns69266:0crwdne69266:0

crwdns69268:0crwdne69268:0 crwdns69270:0crwdne69270:0 crwdns69272:0crwdne69272:0

### crwdns69274:0crwdne69274:0

crwdns69276:0crwdne69276:0 crwdns69278:0crwdne69278:0 crwdns69280:0crwdne69280:0 crwdns69282:0crwdne69282:0 crwdns69284:0crwdne69284:0 crwdns69286:0crwdne69286:0 crwdns69288:0crwdne69288:0 crwdns69290:0crwdne69290:0

```console
crwdns69292:0crwdne69292:0
```

crwdns69294:0crwdne69294:0 crwdns69296:0[package]crwdne69296:0 crwdns69298:0[workspace]crwdne69298:0

<span class="filename">crwdns69300:0crwdne69300:0</span>

```toml
crwdns69302:0crwdne69302:0
```

crwdns69304:0crwdne69304:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-01-adder-crate/add
rm -rf adder
cargo new adder
copy output below
-->

```console
crwdns69306:0crwdne69306:0
```

crwdns69308:0crwdne69308:0 crwdns69310:0crwdne69310:0

```text
crwdns69312:0crwdne69312:0
```

crwdns69314:0crwdne69314:0 crwdns69316:0crwdne69316:0 crwdns69318:0crwdne69318:0 crwdns69320:0crwdne69320:0 crwdns69322:0crwdne69322:0

### crwdns69324:0crwdne69324:0

crwdns69326:0crwdne69326:0 crwdns69328:0crwdne69328:0

<span class="filename">crwdns69330:0crwdne69330:0</span>

```toml
crwdns69332:0crwdne69332:0
```

crwdns69334:0crwdne69334:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-02-add-one/add
rm -rf add_one
cargo new add_one --lib
copy output below
-->

```console
crwdns69336:0crwdne69336:0
```

crwdns69338:0crwdne69338:0

```text
crwdns69340:0crwdne69340:0
```

crwdns69342:0crwdne69342:0

<span class="filename">crwdns69344:0crwdne69344:0</span>

```rust,noplayground
crwdns69346:0crwdne69346:0
```

crwdns69348:0crwdne69348:0 crwdns69350:0crwdne69350:0

<span class="filename">crwdns69352:0crwdne69352:0</span>

```toml
crwdns69354:0crwdne69354:0
```

crwdns69356:0crwdne69356:0

crwdns69358:0crwdne69358:0 crwdns69360:0crwdne69360:0 crwdns69362:0crwdne69362:0

<span class="filename">crwdns69364:0crwdne69364:0</span>

```rust,ignore
crwdns69366:0crwdne69366:0
```


<span class="caption">crwdns69368:0crwdne69368:0</span>

crwdns69370:0crwdne69370:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns69372:0crwdne69372:0
```

crwdns69374:0crwdne69374:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo run -p adder
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns69376:0crwdne69376:0 crwdns69378:0crwdne69378:0
```

crwdns69380:0crwdne69380:0

#### crwdns69382:0crwdne69382:0

crwdns69384:0crwdne69384:0 crwdns69386:0crwdne69386:0 crwdns69388:0crwdne69388:0 crwdns69390:0crwdne69390:0 crwdns69392:0[dependencies]crwdne69392:0


<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch02-00-guessing-game-tutorial.md
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
-->

<span class="filename">crwdns69394:0crwdne69394:0</span>

```toml
crwdns69396:0crwdne69396:0
```

crwdns69398:0crwdne69398:0 crwdns69400:0crwdne69400:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-03-workspace-with-external-dependency/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns69402:0crwdne69402:0
```

crwdns69404:0crwdne69404:0 crwdns69406:0crwdne69406:0 crwdns69408:0crwdne69408:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-03-use-rand/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns69410:0[E0432]crwdne69410:0
```

crwdns69412:0crwdne69412:0 crwdns69414:0crwdne69414:0 crwdns69416:0crwdne69416:0

#### crwdns69418:0crwdne69418:0

crwdns69420:0crwdne69420:0

<span class="filename">crwdns69422:0crwdne69422:0</span>

```rust,noplayground
crwdns69424:0crwdne69424:0
```

crwdns69426:0crwdne69426:0 crwdns69428:0crwdne69428:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test
copy output below; the output updating script doesn't handle subdirectories in
paths properly
-->

```console
crwdns69430:0crwdne69430:0
```

crwdns69432:0crwdne69432:0 crwdns69434:0crwdne69434:0

crwdns69436:0crwdne69436:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test -p add_one
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns69438:0crwdne69438:0
```

crwdns69440:0crwdne69440:0

crwdns69442:0crwdne69442:0 crwdns69444:0crwdne69444:0

crwdns69446:0crwdne69446:0

crwdns69448:0crwdne69448:0 crwdns69450:0crwdne69450:0
