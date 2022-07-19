## crwdns35298:0crwdne35298:0

crwdns35300:0crwdne35300:0 crwdns35302:0crwdne35302:0 crwdns35304:0crwdne35304:0

### crwdns35306:0crwdne35306:0

crwdns35308:0crwdne35308:0 crwdns35310:0crwdne35310:0 crwdns35312:0crwdne35312:0 crwdns35314:0crwdne35314:0 crwdns35316:0crwdne35316:0 crwdns35318:0crwdne35318:0 crwdns35320:0crwdne35320:0 crwdns35322:0crwdne35322:0

```console
crwdns35324:0crwdne35324:0
```

crwdns35326:0crwdne35326:0 crwdns35330:0[package]crwdne35330:0 crwdns35332:0[workspace]crwdne35332:0

<span class="filename">crwdns35338:0crwdne35338:0</span>

```toml
crwdns35340:0crwdne35340:0
```

crwdns35348:0crwdne35348:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-01-adder-crate/add
rm -rf adder
cargo new adder
copy output below
-->

```console
crwdns35350:0crwdne35350:0
```

crwdns35354:0crwdne35354:0 crwdns35356:0crwdne35356:0

```text
crwdns35358:0crwdne35358:0
```

crwdns35364:0crwdne35364:0 crwdns35366:0crwdne35366:0 crwdns35374:0crwdne35374:0 crwdns35376:0crwdne35376:0 crwdns35380:0crwdne35380:0

### crwdns35384:0crwdne35384:0

crwdns35386:0crwdne35386:0 crwdns35388:0crwdne35388:0

<span class="filename">crwdns35390:0crwdne35390:0</span>

```toml
crwdns35392:0crwdne35392:0
```

crwdns35394:0crwdne35394:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-02-add-one/add
rm -rf add_one
cargo new add_one --lib
copy output below
-->

```console
crwdns35396:0crwdne35396:0
```

crwdns35400:0crwdne35400:0

```text
crwdns35406:0crwdne35406:0
```

crwdns35412:0crwdne35412:0

<span class="filename">crwdns35416:0crwdne35416:0</span>

```rust,noplayground
crwdns35426:0crwdne35426:0
```

crwdns35430:0crwdne35430:0 crwdns35434:0crwdne35434:0

<span class="filename">crwdns35438:0crwdne35438:0</span>

```toml
crwdns35440:0crwdne35440:0
```

crwdns35444:0crwdne35444:0

crwdns35448:0crwdne35448:0 crwdns35452:0crwdne35452:0 crwdns35454:0crwdne35454:0

<span class="filename">crwdns35460:0crwdne35460:0</span>

```rust,ignore
crwdns35462:0crwdne35462:0
```


<span class="caption">crwdns35464:0crwdne35464:0</span>

crwdns35466:0crwdne35466:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns35468:0crwdne35468:0
```

crwdns35474:0crwdne35474:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo run -p adder
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns35476:0crwdne35476:0 crwdns35480:0crwdne35480:0
```

crwdns35482:0crwdne35482:0

#### crwdns35486:0crwdne35486:0

crwdns35490:0crwdne35490:0 crwdns35502:0crwdne35502:0 crwdns35506:0crwdne35506:0 crwdns35512:0crwdne35512:0 crwdns35516:0[dependencies]crwdne35516:0


<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch02-00-guessing-game-tutorial.md
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
-->

<span class="filename">crwdns35518:0crwdne35518:0</span>

```toml
crwdns35522:0crwdne35522:0
```

crwdns35528:0crwdne35528:0 crwdns35530:0crwdne35530:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-03-workspace-with-external-dependency/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns35536:0crwdne35536:0
```

crwdns35540:0crwdne35540:0 crwdns35548:0crwdne35548:0 crwdns35550:0crwdne35550:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-03-use-rand/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns35552:0[E0432]crwdne35552:0
```

crwdns35556:0crwdne35556:0 crwdns35558:0crwdne35558:0 crwdns35562:0crwdne35562:0

#### crwdns35564:0crwdne35564:0

crwdns35568:0crwdne35568:0

<span class="filename">crwdns35574:0crwdne35574:0</span>

```rust,noplayground
crwdns35582:0crwdne35582:0
```

crwdns35588:0crwdne35588:0 crwdns35590:0crwdne35590:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test
copy output below; the output updating script doesn't handle subdirectories in
paths properly
-->

```console
crwdns35596:0crwdne35596:0
```

crwdns35598:0crwdne35598:0 crwdns35602:0crwdne35602:0

crwdns35608:0crwdne35608:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test -p add_one
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns35612:0crwdne35612:0
```

crwdns35614:0crwdne35614:0

crwdns35618:0crwdne35618:0 crwdns35622:0crwdne35622:0

crwdns35626:0crwdne35626:0

crwdns35630:0crwdne35630:0 crwdns35634:0crwdne35634:0
