# Gwt Resty
#### Add GWT RESTY and JAX-RS dependency to maven pom.xml file:
```
    <dependency>
         <groupId>org.fusesource.restygwt</groupId>
    	 <artifactId>restygwt</artifactId>
     	 <version>2.0.3</version>
    </dependency>
    <dependency>
        <groupId>javax.ws.rs</groupId>
        <artifactId>javax.ws.rs-api</artifactId>
        <version>2.0</version>
    </dependency>
```
#### Add RestyGWT module to GWT shared module
```
<inherits name="org.fusesource.restygwt.RestyGWT"/>
```
####  Create REST endpoint that return JSON object
```
@RestController
@RequestMapping("/json/expenses")
@Transactional
public class ExpenseRestController {
    ...

    @RequestMapping(value="/get-all", method = RequestMethod.GET, produces="application/json")
    public List<Expense> getAllExpenses() {
        return expenseDao.getAll();
    }

    @RequestMapping(value="/get-range/{start}/{end}", method = RequestMethod.GET, produces="application/json")
    public List<Expense> getExpensesByRange(@PathVariable("start") int start,
                                            @PathVariable("end") int end) {
        return expenseDao.getByRange(start, end);
    }

    @RequestMapping(value="/save-all", method=RequestMethod.PUT, consumes="application/json")
    @ResponseStatus(HttpStatus.OK)
    public void saveAllExpenses(@RequestBody List<Expense> expenses) {
        expenseDao.saveAll(expenses);
    }

}
```
####  Create DTO object in shared module, it will be used by client and server
* @JsonCreater before constructor
* @JsonProperty before constructor parameters
```
public class ExpenseDto {

    private Integer id;
    private String title;
    private BigDecimal amount;
    private String shortDate;
    private CategoryDto category;

    @JsonCreator
    public ExpenseDto(@JsonProperty("id") Integer id,
                      @JsonProperty("title") String title,
                      @JsonProperty("amount") BigDecimal amount,
                      @JsonProperty("shortDate") String shortDate,
                      @JsonProperty("category") CategoryDto categoryDto) {
        this.id = id;
        this.title = title;
        this.amount = amount;
        this.shortDate = shortDate;
        this.category = categoryDto;
    }

    @JsonCreator
    public ExpenseDto(@JsonProperty("title")  String title,
                      @JsonProperty("amount") BigDecimal amount,
                      @JsonProperty("shortDate") String shortDate,
                      @JsonProperty("category") CategoryDto category) {
        this.title = title;
        this.amount = amount;
        this.shortDate = shortDate;
        this.category = category;
    }

    // getters and setters
}
```
####  Create Service interface that extends RestService
* in shared module (e.g. services package)
* @Path indicate the url of REST endpoint
```
@Path("/json/expenses")
public interface ExpenseService extends RestService {
    @GET
    @Path("/get-all")
    void getAll(MethodCallback<List<ExpenseDto>> expenses);

    @GET
    @Path("/get-range/{start}/{end}")
    void getByRange(@PathParam("start") int start,
                    @PathParam("end") int length,
                    MethodCallback<List<ExpenseDto>> methodCallback);

    @PUT
    @Path("/save-all")
    void setAll(List<ExpenseDto> expenses, MethodCallback<Void> callback);
}
```
####  On client side create service object and execute service method
```
Defaults.setServiceRoot(GWT.getHostPageBaseURL());
ExpenseService expenseService = GWT.create(ExpenseService.class);

expenseService.getByRange(start, length, new MethodCallback<List<ExpenseDto>>() {
    @Override
    public void onFailure(Method method, Throwable exception) {
        throw new RuntimeException("Call to server failed: " + exception.getMessage());
    }

    @Override
    public void onSuccess(Method method, List<ExpenseDto> response) {
        // do something with response
    }
});
```

For more details go to RestyGWT home page: [resty-gwt.github.io](https://resty-gwt.github.io/)
