## crwdns19314:0crwdne19314:0

crwdns19316:0crwdne19316:0 crwdns19318:0crwdne19318:0 crwdns19320:0crwdne19320:0 crwdns19322:0crwdne19322:0

### crwdns19324:0crwdne19324:0

crwdns19326:0crwdne19326:0 crwdns19328:0crwdne19328:0

<span class="filename">crwdns19330:0crwdne19330:0</span>

```rust,no_run
crwdns19332:0crwdne19332:0
```


<span class="caption">crwdns19334:0crwdne19334:0</span>

crwdns19336:0crwdne19336:0 crwdns19338:0crwdne19338:0

crwdns19340:0crwdne19340:0 crwdns19342:0crwdne19342:0 crwdns19344:0crwdne19344:0 crwdns19346:0crwdne19346:0

crwdns19348:0crwdne19348:0

crwdns19350:0crwdne19350:0 crwdns19352:0crwdne19352:0 crwdns19354:0crwdne19354:0 crwdns19356:0crwdne19356:0

crwdns19358:0crwdne19358:0

### crwdns19360:0crwdne19360:0

crwdns19362:0crwdne19362:0 crwdns19364:0crwdne19364:0 crwdns19366:0crwdne19366:0 crwdns19368:0crwdne19368:0 crwdns19370:0crwdne19370:0

crwdns19372:0crwdne19372:0

crwdns19374:0crwdne19374:0 crwdns19376:0crwdne19376:0 crwdns19378:0crwdne19378:0 crwdns19380:0crwdne19380:0 crwdns19382:0crwdne19382:0 crwdns19384:0crwdne19384:0

crwdns19386:0crwdne19386:0 crwdns19388:0crwdne19388:0 crwdns19390:0crwdne19390:0

crwdns19392:0crwdne19392:0 crwdns19394:0crwdne19394:0 crwdns19396:0crwdne19396:0

crwdns19398:0crwdne19398:0 crwdns19400:0crwdne19400:0 crwdns19402:0crwdne19402:0

<!-- Old headings. Do not remove or links may break. -->
<a id="code-structure-if-we-could-spawn-a-thread-for-each-request"></a>

#### crwdns19404:0crwdne19404:0

crwdns19406:0crwdne19406:0 crwdns19408:0crwdne19408:0 crwdns19410:0crwdne19410:0 crwdns19412:0crwdne19412:0

<span class="filename">crwdns19414:0crwdne19414:0</span>

```rust,no_run
crwdns19416:0crwdne19416:0
```


<span class="caption">crwdns19418:0crwdne19418:0</span>

crwdns19420:0crwdne19420:0 crwdns19422:0crwdne19422:0 crwdns19424:0crwdne19424:0

<!-- Old headings. Do not remove or links may break. -->
<a id="creating-a-similar-interface-for-a-finite-number-of-threads"></a>

#### crwdns19426:0crwdne19426:0

crwdns19428:0crwdne19428:0 crwdns19430:0crwdne19430:0

<span class="filename">crwdns19432:0crwdne19432:0</span>

```rust,ignore,does_not_compile
crwdns19434:0crwdne19434:0
```

<span class="caption">crwdns19436:0crwdne19436:0</span>

crwdns19438:0crwdne19438:0 crwdns19440:0crwdne19440:0 crwdns19442:0crwdne19442:0 crwdns19444:0crwdne19444:0

<!-- Old headings. Do not remove or links may break. -->
<a id="building-the-threadpool-struct-using-compiler-driven-development"></a>

#### crwdns19446:0crwdne19446:0

crwdns19448:0crwdne19448:0 crwdns19450:0crwdne19450:0

```console
crwdns19452:0crwdne19452:0
```

crwdns19454:0crwdne19454:0 crwdns19456:0crwdne19456:0 crwdns19458:0crwdne19458:0 crwdns19460:0crwdne19460:0 crwdns19462:0crwdne19462:0

crwdns19464:0crwdne19464:0

<span class="filename">crwdns19466:0crwdne19466:0</span>

```rust,noplayground
crwdns19468:0crwdne19468:0
```

crwdns19470:0crwdne19470:0

<span class="filename">crwdns19472:0crwdne19472:0</span>

```rust,ignore
crwdns19474:0crwdne19474:0
```

crwdns19476:0crwdne19476:0

```console
crwdns19478:0crwdne19478:0
```

crwdns19480:0crwdne19480:0 crwdns19482:0crwdne19482:0 crwdns19484:0crwdne19484:0

<span class="filename">crwdns19486:0crwdne19486:0</span>

```rust,noplayground
crwdns19488:0crwdne19488:0
```

crwdns19490:0crwdne19490:0 crwdns19492:0crwdne19492:0<!--
ignore --> crwdns19494:0crwdne19494:0

crwdns19496:0crwdne19496:0

```console
crwdns19498:0crwdne19498:0
```

crwdns19500:0crwdne19500:0 crwdns19502:0crwdne19502:0<!-- ignore --> crwdns19504:0crwdne19504:0 crwdns19506:0crwdne19506:0

crwdns19508:0crwdne19508:0 crwdns19510:0crwdne19510:0<!-- ignore --> crwdns19512:0crwdne19512:0 crwdns19514:0crwdne19514:0 crwdns19516:0crwdne19516:0 crwdns19518:0crwdne19518:0

```rust,ignore
crwdns19520:0crwdne19520:0
```

crwdns19522:0crwdne19522:0 crwdns19524:0crwdne19524:0 crwdns19526:0crwdne19526:0 crwdns19528:0crwdne19528:0

crwdns19530:0crwdne19530:0 crwdns19532:0crwdne19532:0

<span class="filename">crwdns19534:0crwdne19534:0</span>

```rust,noplayground
crwdns19536:0crwdne19536:0
```

crwdns19538:0crwdne19538:0 crwdns19540:0crwdne19540:0

crwdns19542:0crwdne19542:0 crwdns19544:0crwdne19544:0

```console
crwdns19546:0crwdne19546:0
```

crwdns19548:0crwdne19548:0 crwdns19550:0crwdne19550:0 crwdns19552:0crwdne19552:0

> crwdns19554:0crwdne19554:0 crwdns19556:0crwdne19556:0 crwdns19558:0crwdne19558:0 crwdns19560:0crwdne19560:0

#### crwdns19562:0crwdne19562:0

crwdns19564:0crwdne19564:0 crwdns19566:0crwdne19566:0 crwdns19568:0crwdne19568:0 crwdns19570:0crwdne19570:0 crwdns19572:0crwdne19572:0 crwdns19574:0crwdne19574:0

<span class="filename">crwdns19576:0crwdne19576:0</span>

```rust,noplayground
crwdns19578:0crwdne19578:0
```


<span class="caption">crwdns19580:0crwdne19580:0</span>

crwdns19582:0crwdne19582:0 crwdns19584:0crwdne19584:0 crwdns19586:0crwdne19586:0

crwdns19588:0crwdne19588:0 crwdns19590:0crwdne19590:0 crwdns19592:0crwdne19592:0

```rust,ignore
crwdns19594:0crwdne19594:0
```

#### crwdns19596:0crwdne19596:0

crwdns19598:0crwdne19598:0 crwdns19600:0crwdne19600:0 crwdns19602:0crwdne19602:0

```rust,ignore
crwdns19604:0crwdne19604:0
```

crwdns19606:0crwdne19606:0 crwdns19608:0crwdne19608:0 crwdns19610:0crwdne19610:0

crwdns19612:0crwdne19612:0 crwdns19614:0crwdne19614:0

<span class="filename">crwdns19616:0crwdne19616:0</span>

```rust,ignore,not_desired_behavior
crwdns19618:0crwdne19618:0
```


<span class="caption">crwdns19620:0crwdne19620:0</span>

crwdns19622:0crwdne19622:0

crwdns19624:0crwdne19624:0 crwdns19626:0crwdne19626:0 crwdns19628:0crwdne19628:0

crwdns19630:0crwdne19630:0

#### crwdns19632:0crwdne19632:0

crwdns19634:0crwdne19634:0 crwdns19636:0crwdne19636:0 crwdns19638:0crwdne19638:0 crwdns19640:0crwdne19640:0 crwdns19642:0crwdne19642:0

crwdns19644:0crwdne19644:0 crwdns19646:0crwdne19646:0 crwdns19648:0crwdne19648:0 crwdns19650:0crwdne19650:0

crwdns19652:0crwdne19652:0 crwdns19654:0crwdne19654:0 crwdns19656:0crwdne19656:0 crwdns19658:0crwdne19658:0

crwdns19660:0crwdne19660:0 crwdns19662:0crwdne19662:0

1. crwdns19664:0crwdne19664:0
2. crwdns19666:0crwdne19666:0
3. crwdns19668:0crwdne19668:0
4. crwdns19670:0crwdne19670:0

crwdns19672:0crwdne19672:0

crwdns19674:0crwdne19674:0 crwdns19676:0crwdne19676:0

<span class="filename">crwdns19678:0crwdne19678:0</span>

```rust,noplayground
crwdns19680:0crwdne19680:0
```


<span class="caption">crwdns19682:0crwdne19682:0</span>

crwdns19684:0crwdne19684:0 crwdns19686:0crwdne19686:0

crwdns19688:0crwdne19688:0 crwdns19690:0crwdne19690:0

> crwdns19692:0crwdne19692:0 crwdns19694:0crwdne19694:0 crwdns19696:0:thread:crwdne19696:0<!-- ignore --> crwdns19698:0crwdne19698:0<!-- ignore --> crwdns19700:0crwdne19700:0

crwdns19702:0crwdne19702:0 crwdns19704:0crwdne19704:0 crwdns19706:0crwdne19706:0

#### crwdns19708:0crwdne19708:0

crwdns19710:0crwdne19710:0 crwdns19712:0crwdne19712:0 crwdns19714:0crwdne19714:0

crwdns19716:0crwdne19716:0

crwdns19718:0crwdne19718:0 crwdns19720:0crwdne19720:0 crwdns19722:0crwdne19722:0

1. crwdns19724:0crwdne19724:0
2. crwdns19726:0crwdne19726:0
3. crwdns19728:0crwdne19728:0
4. crwdns19730:0crwdne19730:0
5. crwdns19732:0crwdne19732:0

crwdns19734:0crwdne19734:0 crwdns19736:0crwdne19736:0

<span class="filename">crwdns19738:0crwdne19738:0</span>

```rust,noplayground
crwdns19740:0crwdne19740:0
```


<span class="caption">crwdns19742:0crwdne19742:0</span>

crwdns19744:0crwdne19744:0 crwdns19746:0crwdne19746:0

crwdns19748:0crwdne19748:0 crwdns19750:0crwdne19750:0 crwdns19752:0crwdne19752:0

<span class="filename">crwdns19754:0crwdne19754:0</span>

```rust,ignore,does_not_compile
crwdns19756:0crwdne19756:0
```

<span class="caption">crwdns19758:0crwdne19758:0</span>

crwdns19760:0crwdne19760:0

crwdns19762:0crwdne19762:0

```console
crwdns19764:0crwdne19764:0
```

crwdns19766:0crwdne19766:0 crwdns19768:0crwdne19768:0 crwdns19770:0crwdne19770:0 crwdns19772:0crwdne19772:0

crwdns19774:0crwdne19774:0

crwdns19776:0crwdne19776:0 crwdns19778:0crwdne19778:0 crwdns19780:0crwdne19780:0

<span class="filename">crwdns19782:0crwdne19782:0</span>

```rust,noplayground
crwdns19784:0crwdne19784:0
```


<span class="caption">crwdns19786:0crwdne19786:0</span>

crwdns19788:0crwdne19788:0 crwdns19790:0crwdne19790:0

crwdns19792:0crwdne19792:0 crwdns19794:0crwdne19794:0

#### crwdns19796:0crwdne19796:0

crwdns19798:0crwdne19798:0 crwdns19800:0crwdne19800:0 crwdns19802:0crwdne19802:0<!-- ignore -->
crwdns19804:0crwdne19804:0 crwdns19806:0crwdne19806:0

<span class="filename">crwdns19808:0crwdne19808:0</span>

```rust,noplayground
crwdns19810:0crwdne19810:0
```


<span class="caption">crwdns19812:0crwdne19812:0</span>

crwdns19814:0crwdne19814:0 crwdns19816:0crwdne19816:0 crwdns19818:0crwdne19818:0 crwdns19820:0crwdne19820:0 crwdns19822:0crwdne19822:0

crwdns19824:0crwdne19824:0 crwdns19826:0crwdne19826:0 crwdns19828:0crwdne19828:0 crwdns19830:0crwdne19830:0

<span class="filename">crwdns19832:0crwdne19832:0</span>

```rust,noplayground
crwdns19834:0crwdne19834:0
```


<span class="caption">crwdns19836:0crwdne19836:0</span>

crwdns19838:0crwdne19838:0 crwdns19840:0crwdne19840:0 crwdns19842:0crwdne19842:0 crwdns19844:0crwdne19844:0

crwdns19846:0crwdne19846:0 crwdns19848:0crwdne19848:0

crwdns19850:0crwdne19850:0 crwdns19852:0crwdne19852:0

crwdns19854:0crwdne19854:0 crwdns19856:0crwdne19856:0

<!-- manual-regeneration
cd listings/ch20-web-server/listing-20-20
cargo run
make some requests to 127.0.0.1:7878
Can't automate because the output depends on making requests
-->

```console
crwdns19858:0crwdne19858:0
crwdns19860:0crwdne19860:0
crwdns19862:0crwdne19862:0
crwdns19864:0crwdne19864:0
crwdns19866:0crwdne19866:0
crwdns19868:0crwdne19868:0
crwdns19870:0crwdne19870:0
crwdns19872:0crwdne19872:0
crwdns19874:0crwdne19874:0
crwdns19876:0crwdne19876:0
```

crwdns19878:0crwdne19878:0 crwdns19880:0crwdne19880:0 crwdns19882:0crwdne19882:0 crwdns19884:0crwdne19884:0

> crwdns19886:0crwdne19886:0 crwdns19888:0crwdne19888:0 crwdns19890:0crwdne19890:0

crwdns19892:0crwdne19892:0

<span class="filename">crwdns19894:0crwdne19894:0</span>

```rust,ignore,not_desired_behavior
crwdns19896:0crwdne19896:0
```


<span class="caption">crwdns19898:0crwdne19898:0</span>

crwdns19900:0crwdne19900:0 crwdns19902:0crwdne19902:0 crwdns19904:0crwdne19904:0 crwdns19906:0crwdne19906:0

crwdns19908:0crwdne19908:0 crwdns19910:0crwdne19910:0 crwdns19912:0crwdne19912:0
crwdns19914:0crwdne19914:0
