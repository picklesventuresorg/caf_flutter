## Unit Testing

- **[Introduction](#heading--1)**
- **[Mock Object](#heading--2)**
- **[Unit Test](#heading--3)**
- **[Code Coverage](#heading--4)**

<a id="heading--1"></a>

### Introduction

Key components in unit testing are include:

- Mocking Dependencies: Mock objects are used to simulate external dependencies, ensuring tests focus solely on the behavior of the unit under examination.
- Expectations and Assertions: Tests include expectations and assertions to verify that the unit behaves as expected under various conditions.
- Test Runners: Flutter provides test runners that execute unit tests and report results, allowing developers to identify and address issues promptly.

We'll use the dashboard module as a practical example to demonstrate unit testing with BLoC and MVVM architecture. The dashboard module typically involves asynchronous operations, data fetching, and error handling, making it an ideal candidate for showcasing the effectiveness of unit testing in ensuring the reliability of complex application logic.

In the subsequent sections, we'll walk through creating mock objects, setting up a test helper class, and writing unit tests for the `DashboardService` and `DashboardCubit` components. These examples will provide insights into best practices and strategies for effective unit testing in the boilerplate App.

#### Dependencies
    
    dependencies:
        test: ^1.24.1 // A unit testing framework for Dart.
        bloc_test: ^9.1.3 // A library that provides utilities for testing BLoCs and Cubits.
        mockito: ^5.4.2 // A mocking framework for Dart that allows you to create mock objects for testing.
    


<a id="heading--2"></a>

### Mock Object

To ensure accurate and controlled testing, create mock objects (class name: `mock_obj.dart`) for your BLoCs and services. For instance, consider creating a mock object for your dashboard:

    final DashboardModel mockDashboard = DashboardModel(
        carousel: [],
        auctions: [],
        inspections: [],
    );

This allows you to simulate responses during testing.


#### Test Helper Class With Mockito

Create a file name `test_helper.dart` and add these following code. This code uses the `@GenerateMocks` annotation from the mockito package to generate mock implementations of `DashboardCubit`, `DashboardService`, and `DashboardApi`. These mock objects can be used in unit tests to simulate the behavior of these classes without relying on their actual implementations.

    import 'package:mockito/annotations.dart';

    @GenerateMocks([
        DashboardCubit,
        DashboardService,
        DashboardApi,
    ])
    void main() {}

When you run your test suite or build your project, the code generator processes the files containing the `@GenerateMocks` annotation. It then generates a new file named `test_helper.mocks.dart` (or a similar name based on your setup).

**Why?**

- It saves you from manually creating mock classes for each dependency, reducing boilerplate code and making your tests more concise.
- The generated mocks help isolate the unit being tested, ensuring that the focus is solely on the behavior of the unit without interference from its dependencies.


<a id="heading--3"></a>

### Unit Test

#### Unit Test for Service Class

    void main() {
        // Mock objects
        late MockDashboardApi mockDashboardApi;
        late DashboardService sut; // System Under Test

        // Setup function to initialize mock objects and create the SUT
        setUp(() {
            mockDashboardApi = MockDashboardApi();
            sut = DashboardService(client: mockDashboardApi);
        });

        group('Get Dashboard Data', () {
            test('when success, should return DashboardModel', () async {
                // Arrange: Set up the mock behavior
                when(mockDashboardApi.getDashboard())
                    .thenAnswer((_) async => ResponseData(result: mockDashboard));

                // Act: Call the function to be tested
                final result = await sut.dashboard();

                // Assert: Verify the expected behavior
                verify(mockDashboardApi.getDashboard());
                expect(result, mockDashboard);
            });

            test('when error, should throw DataException', () async {
                // Arrange: Set up the mock behavior to simulate an error
                final dioError = DioException(
                    type: DioExceptionType.badResponse,
                    response: Response(
                    data: MockResponse.errorResponse,
                    statusCode: 400,
                    requestOptions: RequestOptions(path: ''),
                    ),
                    requestOptions: RequestOptions(path: ''),
                );

                //given
                when(mockDashboardApi.getDashboard()).thenThrow(dioError);

                //act
                final result = sut.dashboard();

                // //assert
                expect(() => result, throwsA(isInstanceOf<DataException>()));
            });
        });
    }
    

#### Unit Test for Cubit

    
    void main() {
        // Create mock instances of DashboardService and Repository
        late MockDashboardService mockDashboardService;
        late MockRepository mockRepository;

        // Instantiate the DashboardCubit
        late DashboardCubit sut;

        // Set up the test environment
        setUp(() {
            mockDashboardService = MockDashboardService();
            mockRepository = MockRepository();

            // Initialize the DashboardCubit with mock dependencies
            sut = DashboardCubit(
                dashboardService: mockDashboardService,
                repository: mockRepository,
            );
        });

        // Test the initial state of the DashboardCubit
        test('initial state should be DashboardInit', () {
            expect(sut.state, isA<DashboardInit>());
        });

        // Group of tests for DashboardCubit behavior
        group('DashboardCubit', () {
            // Test: Emit DashboardSuccess when the service call completes successfully
            blocTest<DashboardCubit, DashboardState>(
            'emit DashboardSuccess when service call completes successfully',
            build: () => sut,
            act: (DashboardCubit sut) {
                when(mockDashboardService.dashboard())
                    .thenAnswer((_) async => mockDashboard);
                sut.getDashboard();
            },
            expect: () => [isA<DashboardLoading>(), isA<DashboardSuccess>()],
            );

            // Test: Emit DashboardError when an exception is thrown during the service call
            blocTest<DashboardCubit, DashboardState>(
            'emit DashboardError when an exception is thrown during the service call',
            build: () => sut,
            act: (DashboardCubit sut) {
                when(mockDashboardService.dashboard())
                    .thenThrow(const DataException(message: DataErrorMsg.errorBadRequest));
                sut.getDashboard();
            },
            expect: () => [isA<DashboardLoading>(), isA<DashboardError>()],
            );

            // Test: Emit DashboardError State and validate the error message when an exception is caught in DashboardCubit
            blocTest<DashboardCubit, DashboardState>(
            'emit DashboardError State and validate the error message when an exception is caught in DashboardCubit',
            build: () => sut,
            act: (DashboardCubit sut) {
                when(mockDashboardService.dashboard())
                    .thenThrow(const DataException(message: DataErrorMsg.errorBadRequest));
                sut.getDashboard();
            },
            expect: () => [
                isA<DashboardLoading>(),
                predicate<DashboardError>((value) {
                expect(value.errorMsg, DataErrorMsg.errorBadRequest);
                return true;
                })
            ],
            );
        });

        // Tear down the test environment
        tearDown(() {
            sut.close();
        });
    }


<a id="heading--4"></a>

### Code Coverage

#### Execute Test

Execute tests from command line using this:

```bash
    flutter test
```

#### Generate Test Coverage

Execute `--coverage` flag to generate coverage data during tests

```bash
    make test
    // or
    flutter test --coverage
```

#### View Coverage Report

Execute these following command line to view the visualise report for the test coverage

```bash
    make coverage-report
    // or
    genhtml coverage/lcov.info -o coverage/html && open coverage/html/index.html
```

<i>You do need to install `lcov` and `genhtml` in your local machine before proceeding to view the test coverage report</i>
