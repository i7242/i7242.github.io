+++
title = "Retry by Macro"
hascode = true
date = Date(2022, 03, 20)
rss = "Retry by Macro. Xingyu Yan 2022-03-20."
+++
@def tags = ["julia", "macro"]

# Retry failed request by Julia macro

When build micro services, we call many dependencies to gather data. Most of the time we will have 3 times of exponential backoff retry. In Java, we can create an annotation like `@Retry` to enable auto retry when `RetryableException` was thrown.

We can use Julia's macro to do similar thing:

```julia
macro retriable(f, n=3, base=2)
    quote
        for i in 1:$n
            try
                $f
                break
            catch e
                if i == $n
                    @error "failed all retries..."
                else
                    @warn "failed call dependency, will retry later"
                    sleep($base^i)
                end
            end
        end
    end
end
```

When exception was catched, we will retry it later with given limited times. Expression that calls the dependency service is always required. Simple test:

```julia
test_fun() = rand() ≤ 0.5 ? throw(ArgumentError("test_fun failed")) : println("200")

@retriable test_fun()
# example output
# 200

@retriable test_fun() 2
@retriable test_fun() 2 5
# example output
# ┌ Warning: failed call dependency, will retry later
# └ @ Main ~/Playground/retry_macro.jl:15
# ┌ Error: failed all retries...
# └ @ Main ~/Playground/retry_macro.jl:13
```
