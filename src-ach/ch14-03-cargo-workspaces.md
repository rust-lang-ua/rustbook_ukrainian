## crwdns13210:0crwdne13210:0

crwdns13212:0crwdne13212:0 crwdns13214:0crwdne13214:0 crwdns13216:0crwdne13216:0

### crwdns13218:0crwdne13218:0

crwdns13220:0crwdne13220:0 crwdns13222:0crwdne13222:0 crwdns13224:0crwdne13224:0 crwdns13226:0crwdne13226:0 crwdns13228:0crwdne13228:0 crwdns13230:0crwdne13230:0 crwdns13232:0crwdne13232:0 crwdns13234:0crwdne13234:0

```console
crwdns13236:0crwdne13236:0
```

crwdns13238:0crwdne13238:0 crwdns13240:0[package]crwdne13240:0 crwdns13242:0[workspace]crwdne13242:0

<span class="filename">crwdns13244:0crwdne13244:0</span>

```toml
crwdns13246:0crwdne13246:0
```

crwdns13248:0crwdne13248:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-01-adder-crate/add
rm -rf adder
cargo new adder
copy output below
-->

```console
crwdns13250:0crwdne13250:0
```

crwdns13252:0crwdne13252:0 crwdns13254:0crwdne13254:0

```text
crwdns13256:0crwdne13256:0
```

crwdns13258:0crwdne13258:0 crwdns13260:0crwdne13260:0 crwdns13262:0crwdne13262:0 crwdns13264:0crwdne13264:0 crwdns13266:0crwdne13266:0

### crwdns13268:0crwdne13268:0

crwdns13270:0crwdne13270:0 crwdns13272:0crwdne13272:0

<span class="filename">crwdns13274:0crwdne13274:0</span>

```toml
crwdns13276:0crwdne13276:0
```

crwdns13278:0crwdne13278:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-02-add-one/add
rm -rf add_one
cargo new add_one --lib
copy output below
-->

```console
crwdns13280:0crwdne13280:0
```

crwdns13282:0crwdne13282:0

```text
crwdns13284:0crwdne13284:0
```

crwdns13286:0crwdne13286:0

<span class="filename">crwdns13288:0crwdne13288:0</span>

```rust,noplayground
crwdns13290:0crwdne13290:0
```

crwdns13292:0crwdne13292:0 crwdns13294:0crwdne13294:0

<span class="filename">crwdns13296:0crwdne13296:0</span>

```toml
crwdns13298:0crwdne13298:0
```

crwdns13300:0crwdne13300:0

crwdns13302:0crwdne13302:0 crwdns13304:0crwdne13304:0 crwdns13306:0crwdne13306:0

<span class="filename">crwdns13308:0crwdne13308:0</span>

```rust,ignore
crwdns13310:0crwdne13310:0
```


<span class="caption">crwdns13312:0crwdne13312:0</span>

crwdns13314:0crwdne13314:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns13316:0crwdne13316:0
```

crwdns13318:0crwdne13318:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo run -p adder
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns13320:0crwdne13320:0 crwdns13322:0crwdne13322:0
```

crwdns13324:0crwdne13324:0

#### crwdns13326:0crwdne13326:0

crwdns13328:0crwdne13328:0 crwdns13330:0crwdne13330:0 crwdns13332:0crwdne13332:0 crwdns13334:0crwdne13334:0 crwdns13336:0[dependencies]crwdne13336:0


<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch02-00-guessing-game-tutorial.md
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
-->

<span class="filename">crwdns13338:0crwdne13338:0</span>

```toml
crwdns13340:0crwdne13340:0
```

crwdns13342:0crwdne13342:0 crwdns13344:0crwdne13344:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-03-workspace-with-external-dependency/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns13346:0crwdne13346:0
```

crwdns13348:0crwdne13348:0 crwdns13350:0crwdne13350:0 crwdns13352:0crwdne13352:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-03-use-rand/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns13354:0[E0432]crwdne13354:0
```

crwdns13356:0crwdne13356:0 crwdns13358:0crwdne13358:0 crwdns13360:0crwdne13360:0

#### crwdns13362:0crwdne13362:0

crwdns13364:0crwdne13364:0

<span class="filename">crwdns13366:0crwdne13366:0</span>

```rust,noplayground
crwdns13368:0crwdne13368:0
```

crwdns13370:0crwdne13370:0 crwdns13372:0crwdne13372:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test
copy output below; the output updating script doesn't handle subdirectories in
paths properly
-->

```console
crwdns13374:0crwdne13374:0
```

crwdns13376:0crwdne13376:0 crwdns13378:0crwdne13378:0

crwdns13380:0crwdne13380:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test -p add_one
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns13382:0crwdne13382:0
```

crwdns13384:0crwdne13384:0

crwdns13386:0crwdne13386:0 crwdns13388:0crwdne13388:0

crwdns13390:0crwdne13390:0

crwdns13392:0crwdne13392:0 crwdns13394:0crwdne13394:0
