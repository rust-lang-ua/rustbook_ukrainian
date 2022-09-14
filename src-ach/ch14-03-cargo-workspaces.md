## crwdns89978:0crwdne89978:0

crwdns89980:0crwdne89980:0 crwdns89982:0crwdne89982:0 crwdns89984:0crwdne89984:0

### crwdns89986:0crwdne89986:0

crwdns89988:0crwdne89988:0 crwdns89990:0crwdne89990:0 crwdns89992:0crwdne89992:0 crwdns89994:0crwdne89994:0 crwdns89996:0crwdne89996:0 crwdns89998:0crwdne89998:0 crwdns90000:0crwdne90000:0 crwdns90002:0crwdne90002:0

```console
crwdns90004:0crwdne90004:0
```

crwdns90006:0crwdne90006:0 crwdns90008:0[package]crwdne90008:0 crwdns90010:0[workspace]crwdne90010:0

<span class="filename">crwdns90012:0crwdne90012:0</span>

```toml
crwdns90014:0crwdne90014:0
```

crwdns90016:0crwdne90016:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-01-adder-crate/add
rm -rf adder
cargo new adder
copy output below
-->

```console
crwdns90018:0crwdne90018:0
```

crwdns90020:0crwdne90020:0 crwdns90022:0crwdne90022:0

```text
crwdns90024:0crwdne90024:0
```

crwdns90026:0crwdne90026:0 crwdns90028:0crwdne90028:0 crwdns90030:0crwdne90030:0 crwdns90032:0crwdne90032:0 crwdns90034:0crwdne90034:0

### crwdns90036:0crwdne90036:0

crwdns90038:0crwdne90038:0 crwdns90040:0crwdne90040:0

<span class="filename">crwdns90042:0crwdne90042:0</span>

```toml
crwdns90044:0crwdne90044:0
```

crwdns90046:0crwdne90046:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-02-add-one/add
rm -rf add_one
cargo new add_one --lib
copy output below
-->

```console
crwdns90048:0crwdne90048:0
```

crwdns90050:0crwdne90050:0

```text
crwdns90052:0crwdne90052:0
```

crwdns90054:0crwdne90054:0

<span class="filename">crwdns90056:0crwdne90056:0</span>

```rust,noplayground
crwdns90058:0crwdne90058:0
```

crwdns90060:0crwdne90060:0 crwdns90062:0crwdne90062:0

<span class="filename">crwdns90064:0crwdne90064:0</span>

```toml
crwdns90066:0crwdne90066:0
```

crwdns90068:0crwdne90068:0

crwdns90070:0crwdne90070:0 crwdns90072:0crwdne90072:0 crwdns90074:0crwdne90074:0

<span class="filename">crwdns90076:0crwdne90076:0</span>

```rust,ignore
crwdns90078:0crwdne90078:0
```


<span class="caption">crwdns90080:0crwdne90080:0</span>

crwdns90082:0crwdne90082:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns90084:0crwdne90084:0
```

crwdns90086:0crwdne90086:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo run -p adder
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns90088:0crwdne90088:0 crwdns90090:0crwdne90090:0
```

crwdns90092:0crwdne90092:0

#### crwdns90094:0crwdne90094:0

crwdns90096:0crwdne90096:0 crwdns90098:0crwdne90098:0 crwdns90100:0crwdne90100:0 crwdns90102:0crwdne90102:0 crwdns90104:0[dependencies]crwdne90104:0


<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch02-00-guessing-game-tutorial.md
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
-->

<span class="filename">crwdns90106:0crwdne90106:0</span>

```toml
crwdns90108:0crwdne90108:0
```

crwdns90110:0crwdne90110:0 crwdns90112:0crwdne90112:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-03-workspace-with-external-dependency/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns90114:0crwdne90114:0
```

crwdns90116:0crwdne90116:0 crwdns90118:0crwdne90118:0 crwdns90120:0crwdne90120:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-03-use-rand/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns90122:0[E0432]crwdne90122:0
```

crwdns90124:0crwdne90124:0 crwdns90126:0crwdne90126:0 crwdns90128:0crwdne90128:0

#### crwdns90130:0crwdne90130:0

crwdns90132:0crwdne90132:0

<span class="filename">crwdns90134:0crwdne90134:0</span>

```rust,noplayground
crwdns90136:0crwdne90136:0
```

crwdns90138:0crwdne90138:0 crwdns90140:0crwdne90140:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test
copy output below; the output updating script doesn't handle subdirectories in
paths properly
-->

```console
crwdns90142:0crwdne90142:0
```

crwdns90144:0crwdne90144:0 crwdns90146:0crwdne90146:0

crwdns90148:0crwdne90148:0


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test -p add_one
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
crwdns90150:0crwdne90150:0
```

crwdns90152:0crwdne90152:0

crwdns90154:0crwdne90154:0 crwdns90156:0crwdne90156:0

crwdns90158:0crwdne90158:0

crwdns90160:0crwdne90160:0 crwdns90162:0crwdne90162:0
