# DataPreprocessing.ipynb
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "DataPreprocessing.ipynb",
      "provenance": [],
      "toc_visible": true,
      "authorship_tag": "ABX9TyNOVMKZA938M80elS196j2r",
      "include_colab_link": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/happyrabbit/IntroDataScience/blob/master/Python/DataPreprocessing.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "bKk_xnzN0GIb"
      },
      "source": [
        "# Data Preprocessing Notebook\n",
        "\n",
        "In this notebook, we will show how to use python to preprocess the data. "
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "qSM-naA00-3u",
        "outputId": "d381cff1-ba7e-43f4-f162-96062a20be79",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 235
        }
      },
      "source": [
        "# Load packages\n",
        "import pandas as pd\n",
        "import numpy as np\n",
        "from sklearn.impute import SimpleImputer\n",
        "from sklearn.preprocessing import scale, power_transform\n",
        "from sklearn.feature_selection import VarianceThreshold\n",
        "from scipy import stats\n",
        "from statistics import mean\n",
        "import matplotlib.pyplot as plt\n",
        "from matplotlib.pyplot import hist\n",
        "from sklearn.impute import KNNImputer\n",
        "from mlxtend.plotting import scatterplotmatrix\n",
        "import seaborn as sns\n",
        "\n",
        "# Read data\n",
        "dat = pd.read_csv(\"http://bit.ly/2P5gTw4\")\n",
        "dat[:6]"
      ],
      "execution_count": 35,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>age</th>\n",
              "      <th>gender</th>\n",
              "      <th>income</th>\n",
              "      <th>house</th>\n",
              "      <th>store_exp</th>\n",
              "      <th>online_exp</th>\n",
              "      <th>store_trans</th>\n",
              "      <th>online_trans</th>\n",
              "      <th>Q1</th>\n",
              "      <th>Q2</th>\n",
              "      <th>Q3</th>\n",
              "      <th>Q4</th>\n",
              "      <th>Q5</th>\n",
              "      <th>Q6</th>\n",
              "      <th>Q7</th>\n",
              "      <th>Q8</th>\n",
              "      <th>Q9</th>\n",
              "      <th>Q10</th>\n",
              "      <th>segment</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>57</td>\n",
              "      <td>Female</td>\n",
              "      <td>120963.400958</td>\n",
              "      <td>Yes</td>\n",
              "      <td>529.134363</td>\n",
              "      <td>303.512475</td>\n",
              "      <td>2</td>\n",
              "      <td>2</td>\n",
              "      <td>4</td>\n",
              "      <td>2</td>\n",
              "      <td>1</td>\n",
              "      <td>2</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>2</td>\n",
              "      <td>4</td>\n",
              "      <td>Price</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>63</td>\n",
              "      <td>Female</td>\n",
              "      <td>122008.104950</td>\n",
              "      <td>Yes</td>\n",
              "      <td>478.005781</td>\n",
              "      <td>109.529710</td>\n",
              "      <td>4</td>\n",
              "      <td>2</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>1</td>\n",
              "      <td>2</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>Price</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>59</td>\n",
              "      <td>Male</td>\n",
              "      <td>114202.295294</td>\n",
              "      <td>Yes</td>\n",
              "      <td>490.810731</td>\n",
              "      <td>279.249582</td>\n",
              "      <td>7</td>\n",
              "      <td>2</td>\n",
              "      <td>5</td>\n",
              "      <td>2</td>\n",
              "      <td>1</td>\n",
              "      <td>2</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>Price</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>60</td>\n",
              "      <td>Male</td>\n",
              "      <td>113616.337078</td>\n",
              "      <td>Yes</td>\n",
              "      <td>347.809004</td>\n",
              "      <td>141.669752</td>\n",
              "      <td>10</td>\n",
              "      <td>2</td>\n",
              "      <td>5</td>\n",
              "      <td>2</td>\n",
              "      <td>1</td>\n",
              "      <td>3</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>2</td>\n",
              "      <td>4</td>\n",
              "      <td>Price</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>51</td>\n",
              "      <td>Male</td>\n",
              "      <td>124252.552787</td>\n",
              "      <td>Yes</td>\n",
              "      <td>379.625940</td>\n",
              "      <td>112.237177</td>\n",
              "      <td>4</td>\n",
              "      <td>4</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>1</td>\n",
              "      <td>3</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>2</td>\n",
              "      <td>4</td>\n",
              "      <td>Price</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>5</th>\n",
              "      <td>59</td>\n",
              "      <td>Male</td>\n",
              "      <td>107661.456130</td>\n",
              "      <td>Yes</td>\n",
              "      <td>338.315403</td>\n",
              "      <td>195.687013</td>\n",
              "      <td>4</td>\n",
              "      <td>5</td>\n",
              "      <td>4</td>\n",
              "      <td>2</td>\n",
              "      <td>1</td>\n",
              "      <td>2</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "      <td>Price</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "   age  gender         income house   store_exp  ...  Q7  Q8  Q9  Q10  segment\n",
              "0   57  Female  120963.400958   Yes  529.134363  ...   1   4   2    4    Price\n",
              "1   63  Female  122008.104950   Yes  478.005781  ...   1   4   1    4    Price\n",
              "2   59    Male  114202.295294   Yes  490.810731  ...   1   4   1    4    Price\n",
              "3   60    Male  113616.337078   Yes  347.809004  ...   1   4   2    4    Price\n",
              "4   51    Male  124252.552787   Yes  379.625940  ...   1   4   2    4    Price\n",
              "5   59    Male  107661.456130   Yes  338.315403  ...   1   4   1    4    Price\n",
              "\n",
              "[6 rows x 19 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 35
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "DyG-1ajT00pZ"
      },
      "source": [
        "# 01 Data Cleaning\n",
        "\n",
        "After you load the data, the first thing is to check how many variables are there, the type of variables, the distributions, and data errors. You can get descriptive statistics of the data using `describe()` function:"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "UpZ_WlAsReua",
        "outputId": "88931058-2ee4-4046-c6d3-366d9a150e5d",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 317
        }
      },
      "source": [
        "dat.describe()"
      ],
      "execution_count": 86,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>age</th>\n",
              "      <th>income</th>\n",
              "      <th>store_exp</th>\n",
              "      <th>online_exp</th>\n",
              "      <th>store_trans</th>\n",
              "      <th>online_trans</th>\n",
              "      <th>Q1</th>\n",
              "      <th>Q2</th>\n",
              "      <th>Q3</th>\n",
              "      <th>Q4</th>\n",
              "      <th>Q5</th>\n",
              "      <th>Q6</th>\n",
              "      <th>Q7</th>\n",
              "      <th>Q8</th>\n",
              "      <th>Q9</th>\n",
              "      <th>Q10</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>1000.000000</td>\n",
              "      <td>816.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "      <td>1000.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>mean</th>\n",
              "      <td>38.840000</td>\n",
              "      <td>113543.065222</td>\n",
              "      <td>1356.850523</td>\n",
              "      <td>2120.181187</td>\n",
              "      <td>5.350000</td>\n",
              "      <td>13.546000</td>\n",
              "      <td>3.101000</td>\n",
              "      <td>1.823000</td>\n",
              "      <td>1.992000</td>\n",
              "      <td>2.763000</td>\n",
              "      <td>2.945000</td>\n",
              "      <td>2.448000</td>\n",
              "      <td>3.434000</td>\n",
              "      <td>2.396000</td>\n",
              "      <td>3.085000</td>\n",
              "      <td>2.320000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>std</th>\n",
              "      <td>16.416818</td>\n",
              "      <td>49842.287197</td>\n",
              "      <td>2774.399785</td>\n",
              "      <td>1731.224308</td>\n",
              "      <td>3.695559</td>\n",
              "      <td>7.956959</td>\n",
              "      <td>1.450139</td>\n",
              "      <td>1.168348</td>\n",
              "      <td>1.402106</td>\n",
              "      <td>1.155061</td>\n",
              "      <td>1.284377</td>\n",
              "      <td>1.438529</td>\n",
              "      <td>1.455941</td>\n",
              "      <td>1.154347</td>\n",
              "      <td>1.118493</td>\n",
              "      <td>1.136174</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>min</th>\n",
              "      <td>16.000000</td>\n",
              "      <td>41775.637023</td>\n",
              "      <td>-500.000000</td>\n",
              "      <td>68.817228</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25%</th>\n",
              "      <td>25.000000</td>\n",
              "      <td>85832.393634</td>\n",
              "      <td>204.976456</td>\n",
              "      <td>420.341127</td>\n",
              "      <td>3.000000</td>\n",
              "      <td>6.000000</td>\n",
              "      <td>2.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>2.000000</td>\n",
              "      <td>1.750000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>2.500000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>2.000000</td>\n",
              "      <td>1.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>50%</th>\n",
              "      <td>36.000000</td>\n",
              "      <td>93868.682835</td>\n",
              "      <td>328.980863</td>\n",
              "      <td>1941.855436</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>14.000000</td>\n",
              "      <td>3.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>3.000000</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>2.000000</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>2.000000</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>2.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>75%</th>\n",
              "      <td>53.000000</td>\n",
              "      <td>124572.400926</td>\n",
              "      <td>597.293077</td>\n",
              "      <td>2440.774823</td>\n",
              "      <td>7.000000</td>\n",
              "      <td>20.000000</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>2.000000</td>\n",
              "      <td>3.000000</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>3.000000</td>\n",
              "      <td>4.000000</td>\n",
              "      <td>3.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>max</th>\n",
              "      <td>300.000000</td>\n",
              "      <td>319704.337941</td>\n",
              "      <td>50000.000000</td>\n",
              "      <td>9479.442310</td>\n",
              "      <td>20.000000</td>\n",
              "      <td>36.000000</td>\n",
              "      <td>5.000000</td>\n",
              "      <td>5.000000</td>\n",
              "      <td>5.000000</td>\n",
              "      <td>5.000000</td>\n",
              "      <td>5.000000</td>\n",
              "      <td>5.000000</td>\n",
              "      <td>5.000000</td>\n",
              "      <td>5.000000</td>\n",
              "      <td>5.000000</td>\n",
              "      <td>5.000000</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "               age         income  ...           Q9          Q10\n",
              "count  1000.000000     816.000000  ...  1000.000000  1000.000000\n",
              "mean     38.840000  113543.065222  ...     3.085000     2.320000\n",
              "std      16.416818   49842.287197  ...     1.118493     1.136174\n",
              "min      16.000000   41775.637023  ...     1.000000     1.000000\n",
              "25%      25.000000   85832.393634  ...     2.000000     1.000000\n",
              "50%      36.000000   93868.682835  ...     4.000000     2.000000\n",
              "75%      53.000000  124572.400926  ...     4.000000     3.000000\n",
              "max     300.000000  319704.337941  ...     5.000000     5.000000\n",
              "\n",
              "[8 rows x 16 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 86
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "nrsOcjPk2aeq"
      },
      "source": [
        "You can check missing value and column type quickly using `info()`:"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "08oAgij1S-Bu",
        "outputId": "7f4a482d-1415-45bd-a690-1113c88ff20d",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 459
        }
      },
      "source": [
        "dat.info()"
      ],
      "execution_count": 88,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 1000 entries, 0 to 999\n",
            "Data columns (total 19 columns):\n",
            " #   Column        Non-Null Count  Dtype  \n",
            "---  ------        --------------  -----  \n",
            " 0   age           1000 non-null   int64  \n",
            " 1   gender        1000 non-null   object \n",
            " 2   income        816 non-null    float64\n",
            " 3   house         1000 non-null   object \n",
            " 4   store_exp     1000 non-null   float64\n",
            " 5   online_exp    1000 non-null   float64\n",
            " 6   store_trans   1000 non-null   int64  \n",
            " 7   online_trans  1000 non-null   int64  \n",
            " 8   Q1            1000 non-null   int64  \n",
            " 9   Q2            1000 non-null   int64  \n",
            " 10  Q3            1000 non-null   int64  \n",
            " 11  Q4            1000 non-null   int64  \n",
            " 12  Q5            1000 non-null   int64  \n",
            " 13  Q6            1000 non-null   int64  \n",
            " 14  Q7            1000 non-null   int64  \n",
            " 15  Q8            1000 non-null   int64  \n",
            " 16  Q9            1000 non-null   int64  \n",
            " 17  Q10           1000 non-null   int64  \n",
            " 18  segment       1000 non-null   object \n",
            "dtypes: float64(3), int64(13), object(3)\n",
            "memory usage: 148.6+ KB\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "V_yuZ8Cy2jv-"
      },
      "source": [
        "Are there any problems? Questionnaire response Q1-Q10 seem reasonable, the minimum is 1 and maximum is 5. Recall that the questionnaire score is 1-5. The number of store transactions (`store_trans`) and online transactions (`online_trans`) make sense too. Things to pay attention are:\n",
        "\n",
        "1. There are some missing values.\n",
        "2. There are outliers for store expenses (store_exp). The maximum value is 50000. Who would spend $50000 a year buying clothes?! \n",
        "3. There is a negative value ( -500) in store_exp which is not logical.\n",
        "Someone is 300 years old.\n",
        "4. How to deal with that? Depending on the real situation, if the sample size is large enough, it does not hurt to delete those problematic samples. Here we have 1000 observations. Since marketing survey is usually expensive, it is better to set these values as missing and impute them instead of deleting the rows."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "5XiCErAyS-0i",
        "outputId": "2e55c000-d6ca-44a6-f5c2-60caf879ba59",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 187
        }
      },
      "source": [
        "# set problematic values as missings\n",
        "dat.loc[dat.age > 100, 'age'] = np.nan\n",
        "dat.loc[dat.store_exp < 0, 'store_exp'] = np.nan\n",
        "dat.loc[dat.income.isnull(), 'income'] = np.nan\n",
        "# see the results\n",
        "# some of the values are set as NA\n",
        "dat[['income','age', 'store_exp']].info()"
      ],
      "execution_count": 89,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 1000 entries, 0 to 999\n",
            "Data columns (total 3 columns):\n",
            " #   Column     Non-Null Count  Dtype  \n",
            "---  ------     --------------  -----  \n",
            " 0   income     816 non-null    float64\n",
            " 1   age        999 non-null    float64\n",
            " 2   store_exp  999 non-null    float64\n",
            "dtypes: float64(3)\n",
            "memory usage: 23.6 KB\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "yFT-oAQ35zil"
      },
      "source": [
        "# 02-Missing Value"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "uFFEPPgYmNvZ"
      },
      "source": [
        "## 02.1-Impute missing values with `median`, `mode`, `mean`, or `constant`\n",
        "\n",
        "You can set the imputation strategy using `strategy` argument.\n",
        "\n",
        "- If “`mean`”, then replace missing values using the mean along each column. Can only be used with numeric data.\n",
        "- If “`median`”, then replace missing values using the median along each column. Can only be used with numeric data.\n",
        "- If “`most_frequent`”, then replace missing using the most frequent value along each column. Can be used with strings or numeric data.\n",
        "- If “`constant`”, then replace missing values with fill_value. Can be used with strings or numeric data.\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "LrKNnHghdJnD"
      },
      "source": [
        "impdat = dat[['income','age','store_exp']]\n",
        "imp_mean = SimpleImputer(strategy=\"mean\")\n",
        "imp_mean.fit(impdat)\n",
        "impdat = imp_mean.transform(impdat)"
      ],
      "execution_count": 90,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "2HGrp543lZub",
        "outputId": "0ec86300-6f13-44fd-8416-213e3267ce8b",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 204
        }
      },
      "source": [
        "impdat = pd.DataFrame(data=impdat, columns=[\"income\", \"age\",'store_exp'])\n",
        "impdat.head()"
      ],
      "execution_count": 91,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>income</th>\n",
              "      <th>age</th>\n",
              "      <th>store_exp</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>120963.400958</td>\n",
              "      <td>57.0</td>\n",
              "      <td>529.134363</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>122008.104950</td>\n",
              "      <td>63.0</td>\n",
              "      <td>478.005781</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>114202.295294</td>\n",
              "      <td>59.0</td>\n",
              "      <td>490.810731</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>113616.337078</td>\n",
              "      <td>60.0</td>\n",
              "      <td>347.809004</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>124252.552787</td>\n",
              "      <td>51.0</td>\n",
              "      <td>379.625940</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "          income   age   store_exp\n",
              "0  120963.400958  57.0  529.134363\n",
              "1  122008.104950  63.0  478.005781\n",
              "2  114202.295294  59.0  490.810731\n",
              "3  113616.337078  60.0  347.809004\n",
              "4  124252.552787  51.0  379.625940"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 91
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "O8Maaar6ls1f"
      },
      "source": [
        "Let us replace the columns in `dat` with the imputed columns."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "-GkEeDiKdFSa",
        "outputId": "38d9af33-e12e-4a38-8d92-dd2dba15936d",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 459
        }
      },
      "source": [
        "# replace the columns in `dat` with the imputed columns\n",
        "dat2 = dat.drop(columns = ['income','age','store_exp'])\n",
        "dat_imputed = pd.concat([dat2.reset_index(drop=True), impdat] , axis=1)\n",
        "dat_imputed.info()"
      ],
      "execution_count": 84,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 1000 entries, 0 to 999\n",
            "Data columns (total 19 columns):\n",
            " #   Column        Non-Null Count  Dtype  \n",
            "---  ------        --------------  -----  \n",
            " 0   gender        1000 non-null   object \n",
            " 1   house         1000 non-null   object \n",
            " 2   online_exp    1000 non-null   float64\n",
            " 3   store_trans   1000 non-null   int64  \n",
            " 4   online_trans  1000 non-null   int64  \n",
            " 5   Q1            1000 non-null   int64  \n",
            " 6   Q2            1000 non-null   int64  \n",
            " 7   Q3            1000 non-null   int64  \n",
            " 8   Q4            1000 non-null   int64  \n",
            " 9   Q5            1000 non-null   int64  \n",
            " 10  Q6            1000 non-null   int64  \n",
            " 11  Q7            1000 non-null   int64  \n",
            " 12  Q8            1000 non-null   int64  \n",
            " 13  Q9            1000 non-null   int64  \n",
            " 14  Q10           1000 non-null   int64  \n",
            " 15  segment       1000 non-null   object \n",
            " 16  income        1000 non-null   float64\n",
            " 17  age           1000 non-null   float64\n",
            " 18  store_exp     1000 non-null   float64\n",
            "dtypes: float64(4), int64(12), object(3)\n",
            "memory usage: 148.6+ KB\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "O3aN5h-VmPdA"
      },
      "source": [
        "## 02.2-K-nearest neighbors"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "u74TKXosSCbF",
        "outputId": "fda86125-66a7-49ce-c69d-393d10370f4c",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 204
        }
      },
      "source": [
        "impdat = dat[['income','age','store_exp']]\n",
        "imp_knn = KNNImputer(n_neighbors=2, weights=\"uniform\")\n",
        "impdat = imp_knn.fit_transform(impdat)\n",
        "impdat = pd.DataFrame(data=impdat, columns=[\"income\", \"age\",'store_exp'])\n",
        "impdat.head()"
      ],
      "execution_count": 2,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>income</th>\n",
              "      <th>age</th>\n",
              "      <th>store_exp</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>120963.400958</td>\n",
              "      <td>57.0</td>\n",
              "      <td>529.134363</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>122008.104950</td>\n",
              "      <td>63.0</td>\n",
              "      <td>478.005781</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>114202.295294</td>\n",
              "      <td>59.0</td>\n",
              "      <td>490.810731</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>113616.337078</td>\n",
              "      <td>60.0</td>\n",
              "      <td>347.809004</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>124252.552787</td>\n",
              "      <td>51.0</td>\n",
              "      <td>379.625940</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "          income   age   store_exp\n",
              "0  120963.400958  57.0  529.134363\n",
              "1  122008.104950  63.0  478.005781\n",
              "2  114202.295294  59.0  490.810731\n",
              "3  113616.337078  60.0  347.809004\n",
              "4  124252.552787  51.0  379.625940"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 2
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "AhQnjbDanLc9",
        "outputId": "6635a62a-11c3-4721-d70d-5f81998ad817",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 459
        }
      },
      "source": [
        "# replace the columns in `dat` with the imputed columns\n",
        "dat2 = dat.drop(columns = ['income','age','store_exp'])\n",
        "dat_imputed = pd.concat([dat2.reset_index(drop=True), impdat] , axis=1)\n",
        "dat_imputed.info()"
      ],
      "execution_count": 3,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 1000 entries, 0 to 999\n",
            "Data columns (total 19 columns):\n",
            " #   Column        Non-Null Count  Dtype  \n",
            "---  ------        --------------  -----  \n",
            " 0   gender        1000 non-null   object \n",
            " 1   house         1000 non-null   object \n",
            " 2   online_exp    1000 non-null   float64\n",
            " 3   store_trans   1000 non-null   int64  \n",
            " 4   online_trans  1000 non-null   int64  \n",
            " 5   Q1            1000 non-null   int64  \n",
            " 6   Q2            1000 non-null   int64  \n",
            " 7   Q3            1000 non-null   int64  \n",
            " 8   Q4            1000 non-null   int64  \n",
            " 9   Q5            1000 non-null   int64  \n",
            " 10  Q6            1000 non-null   int64  \n",
            " 11  Q7            1000 non-null   int64  \n",
            " 12  Q8            1000 non-null   int64  \n",
            " 13  Q9            1000 non-null   int64  \n",
            " 14  Q10           1000 non-null   int64  \n",
            " 15  segment       1000 non-null   object \n",
            " 16  income        1000 non-null   float64\n",
            " 17  age           1000 non-null   float64\n",
            " 18  store_exp     1000 non-null   float64\n",
            "dtypes: float64(4), int64(12), object(3)\n",
            "memory usage: 148.6+ KB\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "6IeSdF3S2FTp"
      },
      "source": [
        "# 03-Centering and Scaling\n",
        "\n",
        "Let’s standardize variables `income` and `age` from imputed data `dat_imputed`. \n",
        "\n",
        "- `axis`: axis used to compute the means and standard deviations along. If 0, standardize each column, otherwise(if 1) each row.\n",
        "- `with_mean`: if True, center the data before scaliing\n",
        "- `with_std`: if True, scale the data to unit standard deviation."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "CdcUdSaQ4Kz5"
      },
      "source": [
        "dat_s = dat_imputed[['income', 'age']]\n",
        "dat_sed = scale(dat_s,  axis = 0, with_mean = True, with_std = True)"
      ],
      "execution_count": 4,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "esgP8QrW5lp6"
      },
      "source": [
        "After centering and scaling, the features are with mean 0 and standard deviation 1. "
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "0bmIbbmj4vEK",
        "outputId": "bc3a74b0-d782-47a7-f976-8d31bd2f5796",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 297
        }
      },
      "source": [
        "dat_sed = pd.DataFrame(data=dat_sed, columns=[\"income\", \"age\"])\n",
        "dat_sed.describe()"
      ],
      "execution_count": 8,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>income</th>\n",
              "      <th>age</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>1.000000e+03</td>\n",
              "      <td>1.000000e+03</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>mean</th>\n",
              "      <td>2.660649e-16</td>\n",
              "      <td>-1.370015e-16</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>std</th>\n",
              "      <td>1.000500e+00</td>\n",
              "      <td>1.000500e+00</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>min</th>\n",
              "      <td>-1.501074e+00</td>\n",
              "      <td>-1.391952e+00</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25%</th>\n",
              "      <td>-5.806889e-01</td>\n",
              "      <td>-8.434598e-01</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>50%</th>\n",
              "      <td>-4.062407e-01</td>\n",
              "      <td>-1.730799e-01</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>75%</th>\n",
              "      <td>2.192401e-01</td>\n",
              "      <td>8.629617e-01</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>max</th>\n",
              "      <td>4.250212e+00</td>\n",
              "      <td>1.591604e+01</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "             income           age\n",
              "count  1.000000e+03  1.000000e+03\n",
              "mean   2.660649e-16 -1.370015e-16\n",
              "std    1.000500e+00  1.000500e+00\n",
              "min   -1.501074e+00 -1.391952e+00\n",
              "25%   -5.806889e-01 -8.434598e-01\n",
              "50%   -4.062407e-01 -1.730799e-01\n",
              "75%    2.192401e-01  8.629617e-01\n",
              "max    4.250212e+00  1.591604e+01"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 8
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "hG-0Qzzo58Rv"
      },
      "source": [
        "# 04-Resolve Skewness\n",
        "\n",
        "We can use `sklearn.preprocessing.power_transform` to resolve skewness in the data. Currently, `power_transform` supports the Box-Cox transform and the Yeo-Johnson transform. The optimal parameter for stabilizing variance and minimizing skewness is estimated through maximum likelihood. Let's apply Box-Cox transformation."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "640a6yTF4yal"
      },
      "source": [
        "dat_skew = dat_imputed[['income', 'age']]\n",
        "dat_skew_res = power_transform(dat_skew, method = 'box-cox')"
      ],
      "execution_count": 9,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "_eO4qZWT66r1"
      },
      "source": [
        "dat_skew_res = pd.DataFrame(data=dat_skew_res, columns=[\"income\", \"age\"])"
      ],
      "execution_count": 11,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "9MHDKFfm67iy",
        "outputId": "7f8a8b31-da43-4b3d-a28a-63b4dcf2940f",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 294
        }
      },
      "source": [
        "fig, axs = plt.subplots(2)\n",
        "fig.suptitle('Before (top) and after (bottom) transformation')\n",
        "axs[0].hist(dat_imputed.income)\n",
        "axs[1].hist(dat_skew_res.income)\n",
        "plt.show()"
      ],
      "execution_count": 17,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 2 Axes>"
            ]
          },
          "metadata": {
            "tags": [],
            "needs_background": "light"
          }
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "N5SZoGYd03Lx"
      },
      "source": [
        "# 05-Resolve Outliers\n",
        "\n",
        "Box plot, histogram and some other basic visualizations can be used to initially check whether there are outliers. For example, we can visualize numerical non-survey variables using scatter matrix plot:"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "N0NTaVQI05ui",
        "outputId": "3a81b946-a642-4e81-a7a2-7efffc65a595",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 735
        }
      },
      "source": [
        "# select numerical non-survey data\n",
        "subdat = dat_imputed[[\"age\", \"income\", \"store_exp\", \n",
        "    \"online_exp\", \"store_trans\", \"online_trans\"]]\n",
        "subdat = pd.DataFrame(data=subdat, columns=[\"age\", \"income\", \"store_exp\", \n",
        "    \"online_exp\", \"store_trans\", \"online_trans\"])\n",
        "plts = pd.plotting.scatter_matrix(subdat, alpha=0.2, figsize = (12,12))"
      ],
      "execution_count": 22,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 864x864 with 36 Axes>"
            ]
          },
          "metadata": {
            "tags": [],
            "needs_background": "light"
          }
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "0HpP2KI5458E"
      },
      "source": [
        "Let us use MAD (section 5.5 of the book) to detect ourliers. The result here is slightly different because we use the imputed data `dat_imputed` here.\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "bHbU2xgF4xR_",
        "outputId": "982cabc1-f08f-4556-bd02-1b225502e127",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 34
        }
      },
      "source": [
        "# calculate median of the absolute dispersion for income\n",
        "income = dat_imputed.income\n",
        "ymad = stats.median_absolute_deviation(income)\n",
        "# calculate z-score\n",
        "zs = (income - mean(income))/ymad\n",
        "# count the number of outliers\n",
        "sum(zs > 3.5)"
      ],
      "execution_count": 27,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "51"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 27
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "KZjsGrKhBsUn"
      },
      "source": [
        "# 06-Collinearity\n",
        "\n",
        "Checking correlations is an important part of the exploratory data analysis process. In python, you can visualize correlation structure of a set of predictors using [seaborn](https://seaborn.pydata.org) library."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "in4j3QlvaTh1",
        "outputId": "b5ddce81-bfea-4382-84c8-1f9ac48527e2",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 321
        }
      },
      "source": [
        "# select non-survey numerical variables\n",
        "df = dat_imputed[[\"age\", \"income\", \"store_exp\", \"online_exp\", \"store_trans\", \"online_trans\"]]\n",
        "df = pd.DataFrame(df, columns = [\"age\", \"income\", \"store_exp\", \"online_exp\", \"store_trans\", \"online_trans\"])\n",
        "cor_plot = sns.heatmap(df.corr(), annot = True)"
      ],
      "execution_count": 50,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 2 Axes>"
            ]
          },
          "metadata": {
            "tags": [],
            "needs_background": "light"
          }
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "xx1db51vcbTo"
      },
      "source": [
        "The closer the correlation is to 0, the lighter the color is. Let us write a `findCorrelation()` function to remove a minimum number of predictors to ensure all pairwise correlations are below a certain threshold."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "8pGVYnr5_hBx"
      },
      "source": [
        "## Drop out highly correlated features in Python\n",
        "def findCorrelation(df, cutoff = 0.8):\n",
        "    \"\"\"\n",
        "    Given a numeric pd.DataFrame, this will find highly correlated features,\n",
        "    and return a list of features to remove\n",
        "    params:\n",
        "    - df : pd.DataFrame\n",
        "    - cutoff : correlation threshold, will remove one of pairs of features with\n",
        "               a correlation greater than this value\n",
        "    \"\"\"\n",
        "    # Create correlation matrix\n",
        "    corr_matrix = df.corr().abs()\n",
        "    # Select upper triangle of correlation matrix\n",
        "    upper = corr_matrix.where(np.triu(np.ones(corr_matrix.shape), k=1).astype(np.bool))\n",
        "    # Find index of feature columns with correlation greater than cutoff\n",
        "    to_drop = [column for column in upper.columns if any(upper[column] > cutoff)]\n",
        "    return(to_drop)"
      ],
      "execution_count": 51,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "8nVffCXzqSze"
      },
      "source": [
        "Remove the columns:"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "flyLva07c3W5",
        "outputId": "c3f6950f-a702-4065-8c26-dff7039f042c",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 190
        }
      },
      "source": [
        "removeCols = findCorrelation(df, cutoff = 0.7)\n",
        "print(removeCols)\n",
        "df1 = df.drop(columns = removeCols) \n",
        "# check the new cor matrix\n",
        "df1.corr()"
      ],
      "execution_count": 52,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "['store_trans', 'online_trans']\n"
          ],
          "name": "stdout"
        },
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>age</th>\n",
              "      <th>income</th>\n",
              "      <th>store_exp</th>\n",
              "      <th>online_exp</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>age</th>\n",
              "      <td>1.000000</td>\n",
              "      <td>0.286468</td>\n",
              "      <td>0.071618</td>\n",
              "      <td>-0.256114</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>income</th>\n",
              "      <td>0.286468</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>0.594594</td>\n",
              "      <td>0.511080</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>store_exp</th>\n",
              "      <td>0.071618</td>\n",
              "      <td>0.594594</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>0.534909</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>online_exp</th>\n",
              "      <td>-0.256114</td>\n",
              "      <td>0.511080</td>\n",
              "      <td>0.534909</td>\n",
              "      <td>1.000000</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "                 age    income  store_exp  online_exp\n",
              "age         1.000000  0.286468   0.071618   -0.256114\n",
              "income      0.286468  1.000000   0.594594    0.511080\n",
              "store_exp   0.071618  0.594594   1.000000    0.534909\n",
              "online_exp -0.256114  0.511080   0.534909    1.000000"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 52
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "U0z0aRmgsRYZ"
      },
      "source": [
        "# 07-Sparse Variables"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "U3PY33N-vIP7",
        "outputId": "358ef7ca-2677-496c-d76a-bf8a9b8871ff",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 421
        }
      },
      "source": [
        "# create a data frame with sparse variables\n",
        "col1 = [0,0,0,0,1,1,0,0,0,0,0,0,]\n",
        "col2 = range(0,len(col1))\n",
        "a_dict = {\"col1\":col1,  \"col2\":col2}\n",
        "df = pd.DataFrame(a_dict)\n",
        "df"
      ],
      "execution_count": 57,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>col1</th>\n",
              "      <th>col2</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>0</td>\n",
              "      <td>2</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>0</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>1</td>\n",
              "      <td>4</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>5</th>\n",
              "      <td>1</td>\n",
              "      <td>5</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>6</th>\n",
              "      <td>0</td>\n",
              "      <td>6</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>7</th>\n",
              "      <td>0</td>\n",
              "      <td>7</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>8</th>\n",
              "      <td>0</td>\n",
              "      <td>8</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>9</th>\n",
              "      <td>0</td>\n",
              "      <td>9</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>10</th>\n",
              "      <td>0</td>\n",
              "      <td>10</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>11</th>\n",
              "      <td>0</td>\n",
              "      <td>11</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "    col1  col2\n",
              "0      0     0\n",
              "1      0     1\n",
              "2      0     2\n",
              "3      0     3\n",
              "4      1     4\n",
              "5      1     5\n",
              "6      0     6\n",
              "7      0     7\n",
              "8      0     8\n",
              "9      0     9\n",
              "10     0    10\n",
              "11     0    11"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 57
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "zr_kacO9yXIp"
      },
      "source": [
        "Define a function to remove columns that have a low variance. An instance of the class can be created specify the “threshold” argument, which defaults to 0.0 to remove columns with a single value."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "S0jrgkDkuvVj"
      },
      "source": [
        "def variance_threshold_selector(df, threshold=0):\n",
        "    \"\"\"\n",
        "    Given a numeric pd.DataFrame, this will remove columns that have a low variance and the return \n",
        "    the resulted dataframe\n",
        "    params:\n",
        "    - df : input dataframe from which to compute variances.\n",
        "    - threshold : Features with a training set variance lower than this threshold will be removed. \n",
        "                  The default is to keep all features with non-zero variance, i.e. remove the features \n",
        "                  that have the same value in all samples.\n",
        "    \"\"\"\n",
        "    selector = VarianceThreshold(threshold)\n",
        "    selector.fit(df)\n",
        "    res = df[df.columns[selector.get_support(indices=True)]]\n",
        "    return res"
      ],
      "execution_count": 58,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "FI2fhmc2vwQA",
        "outputId": "48afd941-75dd-4d4c-db55-6e1486db75f6",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 421
        }
      },
      "source": [
        "# apply the function to the generated data set\n",
        "variance_threshold_selector(df, threshold = 0.2)"
      ],
      "execution_count": 59,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>col2</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>2</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>4</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>5</th>\n",
              "      <td>5</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>6</th>\n",
              "      <td>6</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>7</th>\n",
              "      <td>7</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>8</th>\n",
              "      <td>8</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>9</th>\n",
              "      <td>9</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>10</th>\n",
              "      <td>10</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>11</th>\n",
              "      <td>11</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "    col2\n",
              "0      0\n",
              "1      1\n",
              "2      2\n",
              "3      3\n",
              "4      4\n",
              "5      5\n",
              "6      6\n",
              "7      7\n",
              "8      8\n",
              "9      9\n",
              "10    10\n",
              "11    11"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 59
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "9S7egB6fsYwB"
      },
      "source": [
        "# 08-Encode Dummy Variables\n",
        "\n",
        "Let’s encode `gender` and `house` from `dat_imputed` to dummy variables. You can use `get_dummies` function from `pandas`:"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "BD__M0vQ0E0X",
        "outputId": "f35083a1-c2c2-4e1e-c2bb-fbbb0d69d38e",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 204
        }
      },
      "source": [
        "df = dat_imputed[['gender', 'house']]\n",
        "pd.get_dummies(df).head()"
      ],
      "execution_count": 62,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>gender_Female</th>\n",
              "      <th>gender_Male</th>\n",
              "      <th>house_No</th>\n",
              "      <th>house_Yes</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "   gender_Female  gender_Male  house_No  house_Yes\n",
              "0              1            0         0          1\n",
              "1              1            0         0          1\n",
              "2              0            1         0          1\n",
              "3              0            1         0          1\n",
              "4              0            1         0          1"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 62
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "jTTPQqz40KK_"
      },
      "source": [
        ""
      ],
      "execution_count": null,
      "outputs": []
    }
  ]
}
