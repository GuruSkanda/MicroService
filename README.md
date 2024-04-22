# MicroService Using Spring Boot

There are 3 services are available in this microservice project. Those are
- movie-catalog-service
- movie-info-service
- ratings-data-service

### How to communicate the micro-services each other?

RestTemplate(It is deprecated for now)
WebClient(It is a part of Reactive Programming)

For example 
```
  RestTemplate restTemplate = new RestTemplate();
  Movie movie = restTemplate.getForObject("http://localhost:8082/movies/"+rating.getMovieId(), Movie.class);
```

WebClient Example
```java
@RestController
@RequestMapping("/catalog")
public class MovieCatalogResource {

    @Autowired
    private RestTemplate restTemplate;

    @Autowired
    private WebClient.Builder webClientBuilder;


    @GetMapping("/{userId}")
    public List<CatalogItem> getCatalog(@PathVariable("userId") String userId){



        List<Rating> ratings = Arrays.asList(
                new Rating("1234", 4),
                new Rating("5678", 4)
        );

        return ratings.stream().map(rating -> {
            //Movie movie = restTemplate.getForObject("http://localhost:8082/movies/"+rating.getMovieId(), Movie.class);
            Movie movie = webClientBuilder.build()
                    .get()
                    .uri("http://localhost:8082/movies/"+rating.getMovieId())
                    .retrieve()
                    .bodyToMono(
                            Movie.class
                    ).block();
            return new CatalogItem(movie.getName(), "Test description", rating.getRating());
        }).collect(Collectors.toList());
    }
}

```

### Don't hard code the URLs. How to solve this?

The answer is Service Discovery
- Discovery Server
- We use Eureka here

### How fault tolerance works
- Solution is to send `heart beats` to service registry.
- If service discovery fails, what to do, the answer is it will search it is the cache, as a developer, you no need to do any cache setup. Bcz it will do by Eureka

## Fault Tolerance And Resilience. 

- Understand challenges with availability. 
- Making micro-services resilient and fault tolerance
- 

