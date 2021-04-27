# outliers
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "#C:\\VKHCG\\05-DS\\4000-UL\\0200-DU\\DU-Outliers.py"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "InputFileName='IP_DATA_CORE.csv'\n",
    "OutputFileName='Retrieve_Router_Location.csv'"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "################################\n",
      "Working Base : C:/VKHCG\n",
      "################################\n"
     ]
    }
   ],
   "source": [
    "Base='C:/VKHCG'\n",
    "print('################################')\n",
    "print('Working Base :',BasesFileName=Base + '/01-Vermeulen/00-RawData/' + InputFileName\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Loading : C:/VKHCG/01-Vermeulen/00-RawData/IP_DATA_CORE.csv\n"
     ]
    }
   ],
   "source": [
    "sFileName=Base + '/01-Vermeulen/00-RawData/' + InputFileName\n",
    "print('Loading :',sFileName)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "All Data\n",
      "     Country Place_Name  Latitude\n",
      "1910      GB     London   51.5130\n",
      "1911      GB     London   51.5508\n",
      "1912      GB     London   51.5649\n",
      "1913      GB     London   51.5895\n",
      "1914      GB     London   51.5232\n",
      "1915      GB     London   51.4739\n",
      "1916      GB     London   51.5491\n",
      "1917      GB     London   51.5085\n",
      "1918      GB     London   51.5085\n",
      "1919      GB     London   51.5161\n",
      "1920      GB     London   51.5198\n",
      "1921      GB     London   51.5198\n",
      "1922      GB     London   51.5085\n",
      "1923      GB     London   51.5237\n",
      "1924      GB     London   51.5237\n",
      "1925      GB     London   51.5237\n",
      "1926      GB     London   51.5237\n",
      "1927      GB     London   51.5232\n",
      "1928      GB     London   51.5085\n",
      "1929      GB     London   51.5085\n",
      "1957      GB     London   51.5115\n",
      "1958      GB     London   51.5092\n",
      "1959      GB     London   51.5092\n",
      "1960      GB     London   51.5092\n",
      "1961      GB     London   51.5092\n",
      "1962      GB     London   51.5092\n",
      "1963      GB     London   51.5092\n",
      "1964      GB     London   51.5092\n",
      "1965      GB     London   51.5092\n",
      "1966      GB     London   51.5092\n",
      "...      ...        ...       ...\n",
      "3409      GB     London   51.5092\n",
      "3410      GB     London   51.5092\n",
      "3411      GB     London   51.5092\n",
      "3412      GB     London   51.5092\n",
      "3413      GB     London   51.5092\n",
      "3414      GB     London   51.5092\n",
      "3415      GB     London   51.5092\n",
      "3416      GB     London   51.5092\n",
      "3417      GB     London   51.5092\n",
      "3418      GB     London   51.5092\n",
      "3419      GB     London   51.5092\n",
      "3420      GB     London   51.5092\n",
      "3421      GB     London   51.5092\n",
      "3422      GB     London   51.5092\n",
      "3423      GB     London   51.5092\n",
      "3424      GB     London   51.5092\n",
      "3425      GB     London   51.5092\n",
      "3426      GB     London   51.5092\n",
      "3427      GB     London   51.5092\n",
      "3428      GB     London   51.5092\n",
      "3429      GB     London   51.5092\n",
      "3430      GB     London   51.5092\n",
      "3431      GB     London   51.5092\n",
      "3432      GB     London   51.5092\n",
      "3433      GB     London   51.5092\n",
      "3434      GB     London   51.5092\n",
      "3435      GB     London   51.5092\n",
      "3436      GB     London   51.5163\n",
      "3437      GB     London   51.5085\n",
      "3438      GB     London   51.5136\n",
      "\n",
      "[1502 rows x 3 columns]\n"
     ]
    }
   ],
   "source": [
    "IP_DATA_ALL=pd.read_csv(sFileName,header=0,low_memory=False,\n",
    "  usecols=['Country','Place Name','Latitude','Longitude'], encoding=\"latin-1\")\n",
    "IP_DATA_ALL.rename(columns={'Place Name': 'Place_Name'}, inplace=True)\n",
    "LondonData=IP_DATA_ALL.loc[IP_DATA_ALL['Place_Name']=='London']\n",
    "AllData=LondonData[['Country', 'Place_Name','Latitude']]\n",
    "print('All Data')\n",
    "print(AllData)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Outliers\n"
     ]
    }
   ],
   "source": [
    "MeanData=AllData.groupby(['Country', 'Place_Name'])['Latitude'].mean()\n",
    "StdData=AllData.groupby(['Country', 'Place_Name'])['Latitude'].std()\n",
    "print('Outliers')\n",
    "UpperBound=float(MeanData+StdData)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Country  Place_Name\n",
       "GB       London        51.509406\n",
       "Name: Latitude, dtype: float64"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "MeanData"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Country  Place_Name\n",
       "GB       London        0.003229\n",
       "Name: Latitude, dtype: float64"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "StdData"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "51.51263550786781"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "UpperBound"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Higher than  51.51263550786781\n",
      "     Country Place_Name  Latitude\n",
      "1910      GB     London   51.5130\n",
      "1911      GB     London   51.5508\n",
      "1912      GB     London   51.5649\n",
      "1913      GB     London   51.5895\n",
      "1914      GB     London   51.5232\n",
      "1916      GB     London   51.5491\n",
      "1919      GB     London   51.5161\n",
      "1920      GB     London   51.5198\n",
      "1921      GB     London   51.5198\n",
      "1923      GB     London   51.5237\n",
      "1924      GB     London   51.5237\n",
      "1925      GB     London   51.5237\n",
      "1926      GB     London   51.5237\n",
      "1927      GB     London   51.5232\n",
      "3436      GB     London   51.5163\n",
      "3438      GB     London   51.5136\n"
     ]
    }
   ],
   "source": [
    "print('Higher than ', UpperBound)\n",
    "OutliersHigher=AllData[AllData.Latitude>UpperBound]\n",
    "print(OutliersHigher)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Lower than  51.50617687562166\n",
      "     Country Place_Name  Latitude\n",
      "1915      GB     London   51.4739\n"
     ]
    }
   ],
   "source": [
    "LowerBound=float(MeanData-StdData)\n",
    "print('Lower than ', LowerBound)\n",
    "OutliersLower=AllData[AllData.Latitude<LowerBound]\n",
    "print(OutliersLower)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Not Outliers\n",
      "     Country Place_Name  Latitude\n",
      "1917      GB     London   51.5085\n",
      "1918      GB     London   51.5085\n",
      "1922      GB     London   51.5085\n",
      "1928      GB     London   51.5085\n",
      "1929      GB     London   51.5085\n",
      "1957      GB     London   51.5115\n",
      "1958      GB     London   51.5092\n",
      "1959      GB     London   51.5092\n",
      "1960      GB     London   51.5092\n",
      "1961      GB     London   51.5092\n",
      "1962      GB     London   51.5092\n",
      "1963      GB     London   51.5092\n",
      "1964      GB     London   51.5092\n",
      "1965      GB     London   51.5092\n",
      "1966      GB     London   51.5092\n",
      "1967      GB     London   51.5092\n",
      "1968      GB     London   51.5092\n",
      "1969      GB     London   51.5092\n",
      "1970      GB     London   51.5092\n",
      "1971      GB     London   51.5092\n",
      "1972      GB     London   51.5092\n",
      "1973      GB     London   51.5092\n",
      "1974      GB     London   51.5092\n",
      "1975      GB     London   51.5092\n",
      "1976      GB     London   51.5092\n",
      "1977      GB     London   51.5092\n",
      "1978      GB     London   51.5092\n",
      "1979      GB     London   51.5092\n",
      "1980      GB     London   51.5092\n",
      "1981      GB     London   51.5092\n",
      "...      ...        ...       ...\n",
      "3407      GB     London   51.5092\n",
      "3408      GB     London   51.5092\n",
      "3409      GB     London   51.5092\n",
      "3410      GB     London   51.5092\n",
      "3411      GB     London   51.5092\n",
      "3412      GB     London   51.5092\n",
      "3413      GB     London   51.5092\n",
      "3414      GB     London   51.5092\n",
      "3415      GB     London   51.5092\n",
      "3416      GB     London   51.5092\n",
      "3417      GB     London   51.5092\n",
      "3418      GB     London   51.5092\n",
      "3419      GB     London   51.5092\n",
      "3420      GB     London   51.5092\n",
      "3421      GB     London   51.5092\n",
      "3422      GB     London   51.5092\n",
      "3423      GB     London   51.5092\n",
      "3424      GB     London   51.5092\n",
      "3425      GB     London   51.5092\n",
      "3426      GB     London   51.5092\n",
      "3427      GB     London   51.5092\n",
      "3428      GB     London   51.5092\n",
      "3429      GB     London   51.5092\n",
      "3430      GB     London   51.5092\n",
      "3431      GB     London   51.5092\n",
      "3432      GB     London   51.5092\n",
      "3433      GB     London   51.5092\n",
      "3434      GB     London   51.5092\n",
      "3435      GB     London   51.5092\n",
      "3437      GB     London   51.5085\n",
      "\n",
      "[1485 rows x 3 columns]\n"
     ]
    }
   ],
   "source": [
    "print('Not Outliers')\n",
    "OutliersNot=AllData[(AllData.Latitude>=LowerBound) & (AllData.Latitude<=UpperBound)]\n",
    "print(OutliersNot)\n",
    "######################"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}

