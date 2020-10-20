# Testing 

### Unit tests

### Integration tests

### System tests

### End to end tests

### TDD

### Test tools


1. Test Rest API using Mockito (unit tests)

```java
import com.skurski.opendata.covid.exception.CovidDataException;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpMethod;
import org.springframework.http.ResponseEntity;
import org.springframework.web.client.RestTemplate;

import static org.assertj.core.api.Assertions.assertThat;
import static org.assertj.core.api.Assertions.catchThrowable;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.ArgumentMatchers.eq;
import static org.mockito.Mockito.when;

@ExtendWith(MockitoExtension.class)
class OpenDataRestClientTest {

    private static final String CSV_DATA_CONTENT = "Test; content; document";
    private static final String OPEN_DATA_ENDPOINT = "https://opendata.ecdc.europa.eu/covid19/casedistribution/csv";

    @Mock
    private RestTemplate restTemplate;

    @InjectMocks
    private OpenDataRestClient openDataRestClient;

    @Test
    void shouldDownloadDocument() {
        byte[] content = CSV_DATA_CONTENT.getBytes();

        when(restTemplate.exchange(
                eq(OPEN_DATA_ENDPOINT),
                eq(HttpMethod.GET),
                any(HttpEntity.class),
                eq(byte[].class)))
                .thenReturn(ResponseEntity.ok(content));

        byte[] document = openDataRestClient.downloadCovidStatistics();

        assertThat(document).isEqualTo(content);
    }

    @Test
    void shouldThrowExceptionWhenResponseIsEmpty() {
        when(restTemplate.exchange(
                eq(OPEN_DATA_ENDPOINT),
                eq(HttpMethod.GET),
                any(HttpEntity.class),
                eq(byte[].class)))
                .thenReturn(ResponseEntity.ok(null));

        Throwable thrown = catchThrowable(() -> openDataRestClient.downloadCovidStatistics());

        assertThat(thrown).isInstanceOf(CovidDataException.class).hasNoCause();
    }
}
```

2. Test Spring MVC Controllers by using @WebMvcTest

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@ExtendWith(SpringExtension.class)
@AutoConfigureMockMvc
@WebMvcTest(controllers = DownloadController.class)
class DownloadControllerTest {

    private static final String DOWNLOAD_CONTROLLER_ENDPOINT = "/covid/download";
    private static final String RESPONSE_MESSAGE = "Covid CSV file successfully downloaded.";

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private OpenDataRestClient openDataRestClient;

    @Test
    void shouldDownloadCsvFile() throws Exception {
        // given
        when(openDataRestClient.downloadCovidStatistics())
                .thenReturn("test;file;content;example".getBytes());

        // when
        mockMvc.perform(
                get(DOWNLOAD_CONTROLLER_ENDPOINT))
                .andDo(print())
                // then
                .andExpect(status().isOk())
                .andExpect(content().string(RESPONSE_MESSAGE));

        verify(openDataRestClient).downloadCovidStatistics();
    }

}
```

3. Integration tests with @SpringBootTest

4. Testing JPA Queries with @DataJpaTest

