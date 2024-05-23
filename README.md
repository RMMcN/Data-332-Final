Final-332
Contributor
Reid McNeill

Introduction
In this project I will analyze the relationship between Lead based Carbon Emissions and Global Average Temperature. I will use Data analytics and R coding language to find a correlation between the two statistics

Cleaning Data
Removing any empty rows

emissions_filtered <- emissions %>%
  filter(Record == "PBA_GgCO2") %>%
  gather(key = "Year", value = "Total_Emissions", -Country, -Record) %>%
  mutate(Total_Emissions = as.numeric(Total_Emissions), Year = as.integer(Year)) %>%
  group_by(Year) %>%
  summarize(Total_Emissions = sum(Total_Emissions, na.rm = TRUE))

  Clean the Temperature Data
temperature_clean <- temperature %>%
  rename(Year = Years, Average_Global_Temp = Temps) %>%
  mutate(Average_Global_Temp = as.numeric(Average_Global_Temp), Year = as.integer(Year))


Merging all the data

df <- merge(emissions_filtered, temperature_clean, by = "Year")

Calculating 5 year average
df_avg <- df %>%
  mutate(Year_Group = (Year - 1970) %/% 5) %>%
  group_by(Year_Group) %>%
  summarize(
    Avg_Total_Emissions = mean(Total_Emissions, na.rm = TRUE),
    Avg_Global_Temp = mean(Average_Global_Temp, na.rm = TRUE)
  ) %>%
  mutate(Year = Year_Group * 5 + 1970)
