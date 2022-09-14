<!-- Old heading. Do not remove or links may break. -->
<a id="closures-anonymous-functions-that-can-capture-their-environment"></a>

## crwdns88798:0crwdne88798:0

crwdns88800:0crwdne88800:0 crwdns88802:0crwdne88802:0 crwdns88804:0crwdne88804:0 crwdns88806:0crwdne88806:0

<!-- Old headings. Do not remove or links may break. -->
<a id="creating-an-abstraction-of-behavior-with-closures"></a>
<a id="refactoring-using-functions"></a>
<a id="refactoring-with-closures-to-store-code"></a>

### crwdns88808:0crwdne88808:0

crwdns88810:0crwdne88810:0 crwdns88812:0crwdne88812:0 crwdns88814:0crwdne88814:0 crwdns88816:0crwdne88816:0 crwdns88818:0crwdne88818:0

crwdns88820:0crwdne88820:0 crwdns88822:0crwdne88822:0 crwdns88824:0crwdne88824:0 crwdns88826:0crwdne88826:0 crwdns88828:0crwdne88828:0

<span class="filename">crwdns88830:0crwdne88830:0</span>

```rust,noplayground
crwdns88832:0crwdne88832:0
```

<span class="caption">crwdns88834:0crwdne88834:0</span>

crwdns88836:0crwdne88836:0 crwdns88838:0crwdne88838:0

crwdns88840:0crwdne88840:0 crwdns88842:0crwdne88842:0 crwdns88844:0crwdne88844:0<!-- ignore --> crwdns88846:0crwdne88846:0 crwdns88848:0crwdne88848:0 crwdns88850:0crwdne88850:0 crwdns88852:0crwdne88852:0

crwdns88854:0crwdne88854:0 crwdns88856:0crwdne88856:0 crwdns88858:0crwdne88858:0 crwdns88860:0crwdne88860:0

crwdns88862:0crwdne88862:0

```console
crwdns88864:0crwdne88864:0
```

crwdns88866:0crwdne88866:0 crwdns88868:0crwdne88868:0 crwdns88870:0crwdne88870:0 crwdns88872:0crwdne88872:0

### crwdns88874:0crwdne88874:0

crwdns88876:0crwdne88876:0 crwdns88878:0crwdne88878:0 crwdns88880:0crwdne88880:0 crwdns88882:0crwdne88882:0 crwdns88884:0crwdne88884:0

crwdns88886:0crwdne88886:0 crwdns88888:0crwdne88888:0

crwdns88890:0crwdne88890:0 crwdns88892:0crwdne88892:0 crwdns88894:0crwdne88894:0

<span class="filename">crwdns88896:0crwdne88896:0</span>

```rust
crwdns88898:0crwdne88898:0
```


<span class="caption">crwdns88900:0crwdne88900:0</span>

crwdns88902:0crwdne88902:0 crwdns88904:0crwdne88904:0 crwdns88906:0crwdne88906:0 crwdns88908:0crwdne88908:0

```rust,ignore
crwdns88910:0crwdne88910:0
```

crwdns88912:0crwdne88912:0 crwdns88914:0crwdne88914:0 crwdns88916:0crwdne88916:0 crwdns88918:0crwdne88918:0 crwdns88920:0crwdne88920:0 crwdns88922:0crwdne88922:0

crwdns88924:0crwdne88924:0 crwdns88926:0crwdne88926:0 crwdns88928:0crwdne88928:0 crwdns88930:0crwdne88930:0 crwdns88932:0crwdne88932:0 crwdns88934:0crwdne88934:0

<span class="filename">crwdns88936:0crwdne88936:0</span>

```rust,ignore,does_not_compile
crwdns88938:0crwdne88938:0
```


<span class="caption">crwdns88940:0crwdne88940:0</span>

crwdns88942:0crwdne88942:0

```console
crwdns88944:0crwdne88944:0
```

crwdns88946:0crwdne88946:0 crwdns88948:0crwdne88948:0

### crwdns88950:0crwdne88950:0

crwdns88952:0crwdne88952:0 crwdns88954:0crwdne88954:0

crwdns88956:0crwdne88956:0

<span class="filename">crwdns88958:0crwdne88958:0</span>

```rust
crwdns88960:0crwdne88960:0
```


<span class="caption">crwdns88962:0crwdne88962:0</span>

crwdns88964:0crwdne88964:0

crwdns88966:0crwdne88966:0 crwdns88968:0crwdne88968:0

```console
crwdns88970:0crwdne88970:0
```

crwdns88972:0crwdne88972:0 crwdns88974:0crwdne88974:0

<span class="filename">crwdns88976:0crwdne88976:0</span>

```rust
crwdns88978:0crwdne88978:0
```


<span class="caption">crwdns88980:0crwdne88980:0</span>

crwdns88982:0crwdne88982:0

```console
crwdns88984:0crwdne88984:0
```

crwdns88986:0crwdne88986:0 crwdns88988:0crwdne88988:0 crwdns88990:0crwdne88990:0 crwdns88992:0crwdne88992:0

crwdns88994:0crwdne88994:0 crwdns88996:0crwdne88996:0 crwdns88998:0crwdne88998:0

<!-- Old headings. Do not remove or links may break. -->
<a id="storing-closures-using-generic-parameters-and-the-fn-traits"></a>
<a id="limitations-of-the-cacher-implementation"></a>
<a id="moving-captured-values-out-of-the-closure-and-the-fn-traits"></a>

### crwdns89000:0crwdne89000:0

crwdns89002:0crwdne89002:0 crwdns89004:0crwdne89004:0

crwdns89006:0crwdne89006:0 crwdns89008:0crwdne89008:0

1. crwdns89010:0crwdne89010:0 crwdns89012:0crwdne89012:0 crwdns89014:0crwdne89014:0
2. crwdns89016:0crwdne89016:0 crwdns89018:0crwdne89018:0
3. crwdns89020:0crwdne89020:0 crwdns89022:0crwdne89022:0

crwdns89024:0crwdne89024:0

```rust,ignore
crwdns89026:0crwdne89026:0
```

crwdns89028:0crwdne89028:0 crwdns89030:0crwdne89030:0

crwdns89032:0crwdne89032:0 crwdns89034:0crwdne89034:0

crwdns89036:0crwdne89036:0 crwdns89038:0crwdne89038:0 crwdns89040:0crwdne89040:0 crwdns89042:0crwdne89042:0 crwdns89044:0crwdne89044:0

> crwdns89046:0crwdne89046:0 crwdns89048:0crwdne89048:0 crwdns89050:0crwdne89050:0

crwdns89052:0crwdne89052:0

crwdns89054:0crwdne89054:0 crwdns89056:0crwdne89056:0 crwdns89058:0crwdne89058:0

<span class="filename">crwdns89060:0crwdne89060:0</span>

```rust
crwdns89062:0crwdne89062:0
```


<span class="caption">crwdns89064:0crwdne89064:0</span>

crwdns89066:0crwdne89066:0

```console
crwdns89068:0crwdne89068:0
```

crwdns89070:0crwdne89070:0 crwdns89072:0crwdne89072:0

crwdns89074:0crwdne89074:0 crwdns89076:0crwdne89076:0

<span class="filename">crwdns89078:0crwdne89078:0</span>

```rust,ignore,does_not_compile
crwdns89080:0crwdne89080:0
```


<span class="caption">crwdns89082:0crwdne89082:0</span>

crwdns89084:0crwdne89084:0 crwdns89086:0crwdne89086:0 crwdns89088:0crwdne89088:0 crwdns89090:0crwdne89090:0 crwdns89092:0crwdne89092:0 crwdns89094:0crwdne89094:0

```console
crwdns89096:0crwdne89096:0
```

crwdns89098:0crwdne89098:0 crwdns89100:0crwdne89100:0 crwdns89102:0crwdne89102:0 crwdns89104:0crwdne89104:0

<span class="filename">crwdns89106:0crwdne89106:0</span>

```rust
crwdns89108:0crwdne89108:0
```


<span class="caption">crwdns89110:0crwdne89110:0</span>

crwdns89112:0crwdne89112:0 crwdns89114:0crwdne89114:0 crwdns89116:0crwdne89116:0
