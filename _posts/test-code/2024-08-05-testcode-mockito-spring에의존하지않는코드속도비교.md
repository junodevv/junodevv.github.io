---
title: "[Mockito] Mockito를 활용한 테스트 코드와 SpringBootTest의 속도비교"
category: TestCode
tags: testCode mockito
---
Mockito 테스트 프레임워크를 사용했을 때의 속도변화

---

# 1. 기존 코드(SpringBootTest, CartServiceTest)
```java
@SpringBootTest
class CartServiceTest {

    @Autowired
    CartService cartService;

    private Long commonMemberId;
    private Long commonProductId;
    private CartDto commonCartDto;

    @BeforeEach
    void setUp() {
        commonMemberId = 1L;
        commonProductId = 1L;
        commonCartDto = CartDto.builder()
                .productId(commonProductId)
                .build();
    }

    @Test
    @DisplayName("장바구니 조회 테스트")
    void 장바구니_조회_테스트() {
        ApiResponse<List<CartDto>> result = cartService.getCartsByMemberId(commonMemberId);

        assertThat(result.getData()).isNotNull();
    }

    @Test
    @DisplayName("장바구니 조회 예외 테스트")
    void 장바구니_조회_예외_테스트_EntityNotFoundException() {
        Long memberId = 1000L;

        assertThatThrownBy(() -> cartService.getCartsByMemberId(memberId))
                .isInstanceOf(EntityNotFoundException.class);
    }

    @Test
    @DisplayName("상품추가 테스트")
    @Transactional
    void 상품추가_테스트() {
        int beforeSize = cartService.getCartsByMemberId(commonMemberId).getData().size();
        CartDto addCartDto = CartDto.builder().productId(5L).build();
        ApiResponse<CartDto> result = cartService.addProduct(addCartDto, commonMemberId);
        int afterSize = cartService.getCartsByMemberId(commonMemberId).getData().size();

        assertThat(afterSize).isEqualTo(beforeSize + 1);
    }

    @ParameterizedTest
    @CsvSource(value = {"1000,1", "1,10000"})
    @DisplayName("상품추가 예외테스트: 사용자 없음(1000), 상품 없음(10000) ")
    void 상품추가_예외테스트_EntityNotFoundException(Long memberId, Long productId) {
        CartDto cartDto = CartDto.builder()
                .productId(productId)
                .build();

        assertThatThrownBy(() -> cartService.addProduct(cartDto, memberId))
                .isInstanceOf(EntityNotFoundException.class);
    }

    @Test
    @DisplayName("상품추가 예외테스트: 중복된 상품")
    @Transactional
    void 상품추가_예외테스트_중복된_상품_ConflictException() {
//        cartService.addProduct(commonCartDto, commonMemberId); // 첫번째 저장

        assertThatThrownBy(() -> cartService.addProduct(commonCartDto, commonMemberId)) // 두번째 저장 -> ConflictException
                .isInstanceOf(ConflictException.class);

    }

    @ParameterizedTest
    @DisplayName("상품 수량 변경 테스트(직접입력)")
    @CsvSource(value = {"6, 6", "5, 5", "1, 1"})
    @Transactional
    void 상품_수량_변경_테스트(Long count, Long expected) {
//        cartService.addProduct(commonCartDto, commonMemberId); // 장바구니에 1L인 상품추가

        ApiResponse<CartDto> response = cartService.updateQuantity(commonProductId, count, commonMemberId);
        Long result = response.getData().getQuantity();

        assertThat(result).isEqualTo(expected);
    }

    @Test
    @DisplayName("상품 수량 변경 테스트(증가)")
    @Transactional
    void 상품_수량_변경_테스트_증가() {
//        cartService.addProduct(commonCartDto, commonMemberId); // 장바구니에 1L인 상품추가

        ApiResponse<CartDto> response = cartService.plusQuantity(commonProductId, commonMemberId);
        Long result = response.getData().getQuantity();

        assertThat(result).isEqualTo(2L);
    }

    @Test
    @DisplayName("상품 수량 변경 테스트(감소)")
    @Transactional
    void 상품_수량_변경_테스트_감소() {
//        cartService.addProduct(commonCartDto, commonMemberId); // 장바구니에 1L인 상품추가
        cartService.updateQuantity(commonProductId, 6L, commonMemberId);

        ApiResponse<CartDto> response = cartService.minusQuantity(commonProductId, commonMemberId);
        Long result = response.getData().getQuantity();

        assertThat(result).isEqualTo(5L);
    }

    @ParameterizedTest
    @ValueSource(longs = {0, -1, -2})
    @DisplayName("상품 수량 변경(직접입력) 예외 테스트: 직접입력한 값이 0이하인 경우")
    @Transactional
    void 상품_수량_변경_예외_테스트_직접입력_IllegalArgumentException(Long count) {
//        cartService.addProduct(commonCartDto, commonMemberId); // 장바구니에 1L인 상품추가, quantity=1

        assertThatThrownBy(() -> cartService.updateQuantity(commonProductId, count, commonMemberId))
                .isInstanceOf(IllegalArgumentException.class);
    }

    @Test
    @DisplayName("상품 수량 변경(감소) 예외 테스트: -한 값이 0 인 경우")
    @Transactional
    void 상품_수량_변경_예외_테스트_감소_IllegalArgumentException() {
//        cartService.addProduct(commonCartDto, commonMemberId); // 장바구니에 1L인 상품추가, quantity=1
        assertThatThrownBy(() -> cartService.minusQuantity(commonProductId, commonMemberId))
                .isInstanceOf(IllegalArgumentException.class);
    }

    @ParameterizedTest
    @CsvSource(value = {"3,1", "1,1000", "100,1"})
    @DisplayName("상품 수량 변경 예외 테스트: 사용자 없음, 상품 없음, 장바구니에 해당 상품없음")
    @Transactional
    void 상품_수량_변경_예외_테스트_EntityNotFoundException(Long memberId, Long productId) {
        Long count = 1L;
        assertThatThrownBy(() -> cartService.updateQuantity(productId, count, memberId))
                .isInstanceOf(EntityNotFoundException.class);
        assertThatThrownBy(() -> cartService.plusQuantity(productId, memberId))
                .isInstanceOf(EntityNotFoundException.class);
        assertThatThrownBy(() -> cartService.minusQuantity(productId, memberId))
                .isInstanceOf(EntityNotFoundException.class);
    }

    @Test
    @DisplayName("(개별)상품 삭제 테스트")
    @Transactional
    void 개별_상품_삭제_테스트() {
//        cartService.addProduct(commonCartDto, commonMemberId);
        int beforeSize = cartService.getCartsByMemberId(commonMemberId).getData().size();

        cartService.deleteProduct(commonProductId, commonMemberId);
        int afterSize = cartService.getCartsByMemberId(commonMemberId).getData().size();

        assertThat(afterSize).isEqualTo(beforeSize - 1);
    }

    @Test
    @DisplayName("(개별)상품 삭제 예외 테스트: 해당 상품이 장바구니에 없는경우")
    @Transactional
    void 개별_상품_삭제_예외_테스트_EntityNotFoundException() {
        Long strangeProductId = 100000L;
        assertThatThrownBy(() -> cartService.deleteProduct(strangeProductId, commonMemberId))
                .isInstanceOf(EntityNotFoundException.class);
    }

    @Test
    @DisplayName("(전체)상품 삭제 테스트")
    @Transactional
    void 전체_상품_삭제_테스트() {
        cartService.deleteAllProduct(commonMemberId);

        int resultSize = cartService.getCartsByMemberId(commonMemberId).getData().size();

        assertThat(resultSize).isEqualTo(0);
    }
}
```

- SpringBootTest로 <b class="text-red">총 21개의 단위테스트</b>가 존재
    - 단 테스트시에는 단위테스트의 갯수를 맞추기 위해 "상품 수량 변경 예외 테스트"는 주석처리후 진행
- 더미 데이터가 변경되면서 이전에는 성공하던 테스트들이 실패하는 경우도 있어 코드의 수정이 필요했다. 즉 더미데이터에 따라 테스트의 결과가 달라진다는 것이다.
- `./gradlew clean test --tests CartServiceTest`로 테스트코드만 빌드 했을때 보통 5초가 소요되었다.
<img width="180" alt="image" src="https://github.com/user-attachments/assets/99b122e8-617f-48a7-bd98-f0098a3c906c">
- 5번 실행했을떄 5s, 5s, 5s, 4s, 5s 로 <b class="text-red">평균 4.8초</b>가 걸렸다.

# 2. 개선된 코드(Mockito, CartServiceTestMock)
```java
@ExtendWith(MockitoExtension.class)
class CartServiceTestMock {

    @Mock
    CartRepository cartRepository;
    @Mock
    MemberRepository memberRepository;
    @Mock
    ProductRepository productRepository;

    @InjectMocks
    CartService cartService;

    private static final Long COMMON_NUMBER = 1L;
    private static final String TEST_PRODUCT_NAME = "테스트 상품1";

    private Long memberId;
    private Long productId;
    private CartDto cartDto;
    private Member member;
    private Product product;
    private Cart cart;

    @BeforeEach
    void setUp() {
        memberId = COMMON_NUMBER;
        productId = COMMON_NUMBER;
        cartDto = CartDto.builder()
                .productId(productId)
                .productName(TEST_PRODUCT_NAME)
                .build();
        product = Product.builder()
                .productId(productId)
                .productName(TEST_PRODUCT_NAME)
                .build();
        cart = Cart.builder()
                .cartId(COMMON_NUMBER)
                .member(member)
                .product(product)
                .quantity(COMMON_NUMBER)
                .build();
        member = new Member();
        member.setMemberId(memberId);
    }

    @Test
    @DisplayName("장바구니 조회 테스트")
    void 장바구니_조회_테스트() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(cartRepository.findByMemberEquals(member)).thenReturn(Arrays.asList(cart));

        ApiResponse<List<CartDto>> result = cartService.getCartsByMemberId(memberId);
        assertThat(result.getData().size()).isEqualTo(1);
    }

    @Test
    @DisplayName("장바구니 조회 예외 테스트")
    void 장바구니_조회_예외_테스트_EntityNotFoundException() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.empty());

        assertThatThrownBy(() -> cartService.getCartsByMemberId(memberId))
                .isInstanceOf(EntityNotFoundException.class);
    }

    @Test
    @DisplayName("상품추가 테스트")
    void 상품추가_테스트() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
        when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(Optional.empty());

        ApiResponse<CartDto> result = cartService.addProduct(cartDto, memberId);

        String expectMessage = String.format(CartMessage.SUCCESS_ADD.getMessage(), TEST_PRODUCT_NAME);
        assertThat(result.getMessage()).isEqualTo(expectMessage);
    }

    @Test
    @DisplayName("상품추가 예외테스트: 사용자 없음")
    void 상품추가_예외테스트_사용자없음_EntityNotFoundException() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.empty());

        assertThatThrownBy(() -> cartService.addProduct(cartDto, memberId))
                .isInstanceOf(EntityNotFoundException.class);
    }

    @Test
    @DisplayName("상품추가 예외테스트: 상품 없음")
    void 상품추가_예외테스트_상품없음_EntityNotFoundException() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.empty());

        assertThatThrownBy(() -> cartService.addProduct(cartDto, memberId))
                .isInstanceOf(EntityNotFoundException.class);
    }

    @Test
    @DisplayName("상품추가 예외테스트: 중복된 상품")
    void 상품추가_예외테스트_중복된_상품_ConflictException() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
        // 이미 존재하는 상황가정
        when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(Optional.ofNullable(cart));

        assertThatThrownBy(() -> cartService.addProduct(cartDto, memberId))
                .isInstanceOf(ConflictException.class);

    }

    @ParameterizedTest
    @DisplayName("상품 수량 변경 테스트(직접입력)")
    @CsvSource(value = {"6, 6", "5, 5", "1, 1"})
    void 상품_수량_변경_테스트(Long count, Long expected) {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
        when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(Optional.ofNullable(cart));

        ApiResponse<CartDto> response = cartService.updateQuantity(productId, count, memberId);
        Long result = response.getData().getQuantity();

        assertThat(result).isEqualTo(expected);
    }

    @Test
    @DisplayName("상품 수량 변경 테스트(증가)")
    void 상품_수량_변경_테스트_증가() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
        when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(Optional.ofNullable(cart));

        ApiResponse<CartDto> response = cartService.plusQuantity(productId, memberId);
        Long result = response.getData().getQuantity();

        assertThat(result).isEqualTo(2L);
    }

    @Test
    @DisplayName("상품 수량 변경 테스트(감소)")
    void 상품_수량_변경_테스트_감소() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
        Cart customCart = Cart.builder()
                .cartId(COMMON_NUMBER)
                .member(member)
                .product(product)
                .quantity(6L)
                .build();
        when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(
                Optional.ofNullable(customCart));

        ApiResponse<CartDto> response = cartService.minusQuantity(productId, memberId);
        Long result = response.getData().getQuantity();

        assertThat(result).isEqualTo(5L);
    }

    @ParameterizedTest
    @ValueSource(longs = {0, -1, -2})
    @DisplayName("상품 수량 변경(직접입력) 예외 테스트: 직접입력한 값이 0이하인 경우")
    void 상품_수량_변경_예외_테스트_직접입력_IllegalArgumentException(Long count) {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
        when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(Optional.ofNullable(cart));

        assertThatThrownBy(() -> cartService.updateQuantity(productId, count, memberId))
                .isInstanceOf(IllegalArgumentException.class);
    }

    @Test
    @DisplayName("상품 수량 변경(감소) 예외 테스트: -한 값이 0 인 경우")
    void 상품_수량_변경_예외_테스트_감소_IllegalArgumentException() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
        when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(Optional.ofNullable(cart));

        assertThatThrownBy(() -> cartService.minusQuantity(productId, memberId))
                .isInstanceOf(IllegalArgumentException.class);
    }

   @Test
   @DisplayName("상품 수량 변경 예외 테스트: 장바구니에 해당 상품없음")
   void 상품_수량_변경_예외_테스트_EntityNotFoundException() {
       when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
       when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
       when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(Optional.empty());

       assertThatThrownBy(() -> cartService.updateQuantity(productId, 1L, memberId))
               .isInstanceOf(EntityNotFoundException.class);
       assertThatThrownBy(() -> cartService.plusQuantity(productId, memberId))
               .isInstanceOf(EntityNotFoundException.class);
       assertThatThrownBy(() -> cartService.minusQuantity(productId, memberId))
               .isInstanceOf(EntityNotFoundException.class);
   }

    @Test
    @DisplayName("(개별)상품 삭제 테스트")
    void 개별_상품_삭제_테스트() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
        when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(Optional.ofNullable(cart));

        ApiResponse<CartDto> result = cartService.deleteProduct(productId, memberId);
        String expectMessage = String.format(CartMessage.SUCCESS_DELETE.getMessage(), TEST_PRODUCT_NAME);

        assertThat(result.getMessage()).isEqualTo(expectMessage);
    }

    @Test
    @DisplayName("(개별)상품 삭제 예외 테스트: 해당 상품이 장바구니에 없는경우")
    void 개별_상품_삭제_예외_테스트_EntityNotFoundException() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));
        when(productRepository.findById(productId)).thenReturn(Optional.ofNullable(product));
        when(cartRepository.findByMemberEqualsAndProductEquals(member, product)).thenReturn(Optional.empty());

        assertThatThrownBy(() -> cartService.deleteProduct(productId, memberId))
                .isInstanceOf(EntityNotFoundException.class);
    }

    @Test
    @DisplayName("(전체)상품 삭제 테스트")
    void 전체_상품_삭제_테스트() {
        when(memberRepository.findById(memberId)).thenReturn(Optional.ofNullable(member));

        ApiResponse<CartDto> result = cartService.deleteAllProduct(memberId);

        assertThat(result.getMessage()).isEqualTo(CartMessage.SUCCESS_DELETE_ALL.getMessage());

    }
}
```

- Mock객체를 이용한 테스트로 <b class="text-red">총 19개의 단위테스트</b> 존재
    - 단 테스트시에는 단위테스트의 갯수를 맞추기 위해 "상품 수량 변경 예외 테스트"는 주석처리후 진행
- 해당 코드 작성후 더미데이터의 변화가 있었지만 계속해서 성공하는 결과를 가져온다.
- `./gradlew clean test --tests CartServiceTestMock`로 테스트코드만 빌드 했을때 보통 2초가 소요되었다.
<img width="180" alt="image" src="https://github.com/user-attachments/assets/3b9e351e-83d7-46d3-8d49-deabcd7d6012">
- 5번 실행했을떄 2s, 1s, 2s, 2s, 2s 로 <b class="text-red">평균 1.8초</b>가 걸렸다.

# 3. 결론
- SpringBootTest는 18개의 단위테스트를 5회 실행했을 때 평균 4.8초의 시간이 걸렸다.
- Mock객체를 이용한 테스트는 18개의 단위테스트를 5회 실행했을 때 평균 1.8초의 시간이 걸렸다.
- 약 Mock객체를 활용한 테스트의 속도가 62.5%정도 빠르다.

### 내생각
테스트코드를 작성하면서도 느꼈지만 확실히 Mock객체를 활용하는 테스트가 빠른것을 알 수 있다.<br>
다만 Repository부분을 내 생각대로 결과를 조작하는 것이기 때문에 실제로 DB와 연결하게 됐을때 내 생각과 결과가 다르다면 잘못된 코드가 될 수 있기에 주의해야할 것 같다.<br>
그렇기 때문에 DB와 연결하는 테스트도 필요하지 않을까 싶기도 하다. 그런 테스트를 위해선 로컬 DB를 따로 구성하거나 하는 방법으로 해결할 수 있을 것 같다.