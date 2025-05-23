Project Graphs
================
2025-04-07

## R Markdown

``` r
library(tidyverse)
library(openintro)
library(ggplot2)
library(scales)
library(plotly)
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

``` r
StateLevelDebt <- read_excel("Project Data Sets/State Level Debt/StateLevelDebt.xlsx")
names(StateLevelDebt) <- gsub(" ", "_", names(StateLevelDebt))
names(StateLevelDebt)
```

    ##  [1] "State_FIPS"                                                                    
    ##  [2] "State_Name"                                                                    
    ##  [3] "State_Abbreviation"                                                            
    ##  [4] "Share_with_any_debt_in_collections,_All"                                       
    ##  [5] "Share_with_any_debt_in_collections,_Comm_of_color"                             
    ##  [6] "Share_with_any_debt_in_collections,_White_comm"                                
    ##  [7] "Median_debt_in_collections,_All"                                               
    ##  [8] "Median_debt_in_collections,_Comm_of_color"                                     
    ##  [9] "Median_debt_in_collections,_White_comm"                                        
    ## [10] "Share_with_medical_debt_in_collections,_All"                                   
    ## [11] "Share_with_medical_debt_in_collections,_Comm_of_color"                         
    ## [12] "Share_with_medical_debt_in_collections,_White_comm"                            
    ## [13] "Share_of_student_loan_holders_with_student_loan_debt_in_default,_All"          
    ## [14] "Share_of_student_loan_holders_with_student_loan_debt_in_default,_Comm_of_color"
    ## [15] "Share_of_student_loan_holders_with_student_loan_debt_in_default,_White_comm"   
    ## [16] "Auto/retail_loan_delinquency_rate,_All"                                        
    ## [17] "Auto/retail_loan_delinquency_rate,_Comm_of_color"                              
    ## [18] "Auto/retail_loan_delinquency_rate,_White_comm"                                 
    ## [19] "Credit_card_debt_delinquency_rate,_All"                                        
    ## [20] "Credit_card_debt_delinquency_rate,_Comm_of_color"                              
    ## [21] "Credit_card_debt_delinquency_rate,_White_comm"                                 
    ## [22] "Median_credit_card_delinquent_debt,_All"                                       
    ## [23] "Median_credit_card_delinquent_debt,_Comm_of_color"                             
    ## [24] "Median_credit_card_delinquent_debt,_White_comm"                                
    ## [25] "Share_of_people_of_color"                                                      
    ## [26] "Average_household_income,_All"                                                 
    ## [27] "Average_household_income,_Comm_of_color"                                       
    ## [28] "Average_household_income,_White_comm"

``` r
names(StateLevelDebt) <- gsub(",", "", names(StateLevelDebt))
names(StateLevelDebt)
```

    ##  [1] "State_FIPS"                                                                   
    ##  [2] "State_Name"                                                                   
    ##  [3] "State_Abbreviation"                                                           
    ##  [4] "Share_with_any_debt_in_collections_All"                                       
    ##  [5] "Share_with_any_debt_in_collections_Comm_of_color"                             
    ##  [6] "Share_with_any_debt_in_collections_White_comm"                                
    ##  [7] "Median_debt_in_collections_All"                                               
    ##  [8] "Median_debt_in_collections_Comm_of_color"                                     
    ##  [9] "Median_debt_in_collections_White_comm"                                        
    ## [10] "Share_with_medical_debt_in_collections_All"                                   
    ## [11] "Share_with_medical_debt_in_collections_Comm_of_color"                         
    ## [12] "Share_with_medical_debt_in_collections_White_comm"                            
    ## [13] "Share_of_student_loan_holders_with_student_loan_debt_in_default_All"          
    ## [14] "Share_of_student_loan_holders_with_student_loan_debt_in_default_Comm_of_color"
    ## [15] "Share_of_student_loan_holders_with_student_loan_debt_in_default_White_comm"   
    ## [16] "Auto/retail_loan_delinquency_rate_All"                                        
    ## [17] "Auto/retail_loan_delinquency_rate_Comm_of_color"                              
    ## [18] "Auto/retail_loan_delinquency_rate_White_comm"                                 
    ## [19] "Credit_card_debt_delinquency_rate_All"                                        
    ## [20] "Credit_card_debt_delinquency_rate_Comm_of_color"                              
    ## [21] "Credit_card_debt_delinquency_rate_White_comm"                                 
    ## [22] "Median_credit_card_delinquent_debt_All"                                       
    ## [23] "Median_credit_card_delinquent_debt_Comm_of_color"                             
    ## [24] "Median_credit_card_delinquent_debt_White_comm"                                
    ## [25] "Share_of_people_of_color"                                                     
    ## [26] "Average_household_income_All"                                                 
    ## [27] "Average_household_income_Comm_of_color"                                       
    ## [28] "Average_household_income_White_comm"

``` r
state_data <- StateLevelDebt |>
  rename(
    State_name = State_Name,
    S_Abbreviation = State_Abbreviation,
    FIPS = State_FIPS,
    
    AnyDebt_All = Share_with_any_debt_in_collections_All,
    AnyDebt_Color = Share_with_any_debt_in_collections_Comm_of_color,
    AnyDebt_White = Share_with_any_debt_in_collections_White_comm,
    
    MedianDebt_All = Median_debt_in_collections_All,
    MedianDebt_Color = Median_debt_in_collections_Comm_of_color,
    MedianDebt_White = Median_debt_in_collections_White_comm,
    
    MedDebt_All = Share_with_medical_debt_in_collections_All,
    MedDebt_Color = Share_with_medical_debt_in_collections_Comm_of_color,
    MedDebt_White = Share_with_medical_debt_in_collections_White_comm,
    
    StudentLoanDefault_All = Share_of_student_loan_holders_with_student_loan_debt_in_default_All,
    StudentLoanDefault_Color = Share_of_student_loan_holders_with_student_loan_debt_in_default_Comm_of_color,
    StudentLoanDefault_White = Share_of_student_loan_holders_with_student_loan_debt_in_default_White_comm,
    
    AutoRetailDelinq_All = `Auto/retail_loan_delinquency_rate_All`,
    AutoRetailDelinq_Color = `Auto/retail_loan_delinquency_rate_Comm_of_color`,
    AutoRetailDelinq_White = `Auto/retail_loan_delinquency_rate_White_comm`,
    
    CreditCardDelinq_All = Credit_card_debt_delinquency_rate_All,
    CreditCardDelinq_Color = Credit_card_debt_delinquency_rate_Comm_of_color,
    CreditCardDelinq_White = Credit_card_debt_delinquency_rate_White_comm,
    
    CreditCardDebt_All = Median_credit_card_delinquent_debt_All,
    CreditCardDebt_Color = Median_credit_card_delinquent_debt_Comm_of_color,
    CreditCardDebt_White = Median_credit_card_delinquent_debt_White_comm,
    
    Percent_POC = Share_of_people_of_color,
    
    Income_All = Average_household_income_All,
    Income_Color = Average_household_income_Comm_of_color,
    Income_White = Average_household_income_White_comm
  )
```

``` r
state_data <- state_data |>
  mutate(
    Percent_POC = as.numeric(Percent_POC),
    Percent_POC = ifelse(Percent_POC > 1, Percent_POC / 100, Percent_POC),
    AnyDebt_C_Percent = as.numeric(AnyDebt_Color),
    AnyDebt_C_Percent = ifelse(AnyDebt_C_Percent > 1, AnyDebt_C_Percent / 100, AnyDebt_C_Percent),
    label_text = paste0(
      "State: ", State_name, "<br>",
      "POC: ", percent(Percent_POC, accuracy = 0.1), "<br>",
      "Debt: ", percent(AnyDebt_C_Percent, accuracy = 0.1)
      )
  )
```

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `AnyDebt_C_Percent = as.numeric(AnyDebt_Color)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion

``` r
p <- ggplot(state_data, aes(
  x = Percent_POC, 
  y = AnyDebt_C_Percent,
  text = label_text)) +
  geom_point(size = 3, alpha = 0.7, stroke = .7, shape = 21, fill = "lightpink", color = "black") +
  scale_x_continuous(labels = percent_format(accuracy = .1),
                     breaks = seq(0, 1, by = 0.1)) +
  scale_y_continuous(labels = percent_format(accuracy = .1),
                     breaks = seq(0, 0.5, by = 0.05))+
  labs(
    title = "Percent of POC vs. Share with Any Debt",
    x = "Percent People of Color",
    y = "Share with Any Debt"
  ) +
  theme_minimal()

ggplotly(p, tooltip = "text")
```

    ## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.

![](Project-starting-graphs_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
state_data <- state_data |>
  mutate(
    loan_all = as.numeric(StudentLoanDefault_All),
    loan_all = ifelse(loan_all > 1, loan_all / 100, loan_all),
    loan_white = as.numeric(StudentLoanDefault_White),
    loan_white = ifelse(loan_white > 1, loan_white / 100, loan_white),
    loan_colored = as.numeric(StudentLoanDefault_Color),
    loan_colored = ifelse(loan_colored > 1, loan_colored / 100, loan_colored)
  )
```

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `loan_colored = as.numeric(StudentLoanDefault_Color)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion

``` r
loan_long <- state_data |>
  select(State_name, loan_all, loan_white, loan_colored) |>
  pivot_longer(
    cols = c(loan_all, loan_white, loan_colored),
    names_to = "Group",
    values_to = "DefaultRate"
  )

ggplot(loan_long, aes(x = Group, y = DefaultRate, fill = Group)) +
         geom_boxplot(alpha = 0.7) +
         scale_y_continuous(labels = percent_format(accuracy = 0.1)) +
         theme_minimal()+
         labs(
           title = "Distribution of Student Loan Default Rates by Group",
           x = "Group",
           y = "Default Rate")
```

    ## Warning: Removed 6 rows containing non-finite values (`stat_boxplot()`).

![](Project-starting-graphs_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## Including Plots
