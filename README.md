There are several assumptions made in this model due to not being able to predict the different interest rates and lack of open source software/ costly prototyping with C 
or C#.
Instead we assume that a person earns a monthly wage after taxes and has to pay a fixed fee each month.
After all the fees are paid, what remains are put into a Swedish savings account that is taxed based on an estimated income.
Each month, the savings gain an interest and so do the fees due to inflation. The growth in earnings however occurs annually.
This factor is proportional to (1 + r)^(1/12), where r is the yearly interest rate and is divided by twelve months.
For simplification, let (1 + r) ^ (1/12) = exp(1/12 * ln(1 + r)) =: D_r.

The Swedish taxes on the ISK is based on a weighted average of capital in each quarter so
YT = (S_3 + S_6 + S_9 + S_12) / 4 * s_r * s, where s_r is the schablo factor and s is the tax rate.
The growth in earnings is quite simple as it is revised each year as
E^1 = E,
E^{i+1} = E^i * (1 + r_e) = E * (1 + r_e)^i, where the ^ indicates indices over the years.
The fees is updated monthly based on inflation.
F_0 = F
F_{i+1} = F_{i} * D_{r_f} = F * D_{r_f}^i

Putting this together, the earnings, fees, and taxes we find that the net savings S_i^j for month i and year j can be calculated recursively using
S_{i+1}^j = S_i^j * D_rs + (E^j-F_i^j)
Where the first term is the interest gained from the previous month, and the second term is the net profit after subtracting earnings from fees with inflation. 
It is easier to define with recursion with the final step being applying taxes during the year's end for i = 12,
S_{i+1}^j  = S_i^j * D_rs + (E^j-F_i^j) - YT^j * I(i==12)

Once a person decides to retire, E = 0, but the savings are still gaining interest after the fees and taxes are paid.
Let YT*^j = (S*_3 + S*_6 + S*_9 + S*_12) / 4 * s_r * s, represent taxes on the remaining savings.

S*_{i+1}^j = S*_i^j * D_rs - F_i^j - YT*^j * I(i==12), here j defines the number of years after retirement instead of years spent working. 

We can notice here that after retirement there is no interest from earnings, only fees and savings.
So starting with S*_i^0 = S_i^j, when S*^j = 0, all the capital has run out but we would like to satisfy the condition that j = (pension age - current age) which implies that 
the regular pension starts to kick in. This is solved with dynamic programming and calculating a matrix based on the initial conditions given by S_i^j.
