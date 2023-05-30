/* 
    Cleaning Data in SQL
    Using Nashville House sale dataset that contains details like Address, Owner details and Sale details.
*/


--Looking at all the Columns

SELECT *
FROM PortfolioProject..NashvilleHousing

-- Standardize Date formate

SELECT SaleDate, CONVERT(Date, SaleDate)
FROM PortfolioProject..NashvilleHousing

UPDATE PortfolioProject..NashvilleHousing
SET SaleDate = CONVERT(Date, SaleDate)

    --OR add another Column with the converted date

ALTER TABLE NashvilleHousing
ADD SaleDateConverted Date;

UPDATE PortfolioProject..NashvilleHousing
SET SaleDateConverted = CONVERT(Date, SaleDate)

--Populate Property Address data

SELECT a.ParcelID,a.PropertyAddress, b.ParcelID, b.PropertyAddress, ISNULL(a.PropertyAddress, b.PropertyAddress)
FROM NashvilleHousing a
JOIN NashvilleHousing b
    ON a.ParcelID = b.ParcelID
    AND a.UniqueID <> b.UniqueID
WHERE a.PropertyAddress IS NULL

UPDATE a
SET PropertyAddress = ISNULL(a.PropertyAddress, b.propertyAddress)
FROM NashvilleHousing a
JOIN NashvilleHousing b
    ON a.ParcelID = b.ParcelID
    AND a.UniqueID <> b.UniqueID
WHERE a.PropertyAddress IS NULL

--Breaking out Address into Individual columns (Address, City, State)
--Spliting PropertyAddress

SELECT PropertyAddress, 
    SUBSTRING(PropertyAddress, 1, CHARINDEX(',',PropertyAddress)-1) AS Address,
    SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress)+1, len(PropertyAddress)) AS City
FROM NashvilleHousing

ALTER TABLE NashvilleHousing
ADD PropertySplitAddress nvarchar(255);

UPDATE PortfolioProject..NashvilleHousing
SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',',PropertyAddress)-1)

ALTER TABLE NashvilleHousing
ADD PropertySplitCity nvarchar(255);

UPDATE PortfolioProject..NashvilleHousing
SET PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress)+1, len(PropertyAddress))

SELECT PropertyAddress, PropertySplitAddress, PropertySplitCity
FROM NashvilleHousing

--Spliting OwnerAddress

SELECT OwnerAddress,
    PARSENAME(REPLACE(OwnerAddress,',','.'),3) AS Address,
    PARSENAME(REPLACE(OwnerAddress,',','.'),2) AS City,
    PARSENAME(REPLACE(OwnerAddress,',','.'),1) AS STATE
FROM NashvilleHousing

ALTER TABLE NashvilleHousing
ADD OwnerSplitAddress nvarchar(255);

ALTER TABLE NashvilleHousing
ADD OwnerSplitCity nvarchar(255);

ALTER TABLE NashvilleHousing
ADD OwnerSplitState nvarchar(255);

UPDATE PortfolioProject..NashvilleHousing
SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress,',','.'),3)

UPDATE PortfolioProject..NashvilleHousing
SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress,',','.'),2)

UPDATE PortfolioProject..NashvilleHousing
SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress,',','.'),1)

SELECT OwnerAddress, OwnerSplitAddress, OwnerSplitCity, OwnerSplitState
FROM NashvilleHousing

--Change Yes and No to Y and N in "Sold as vacant" field

SELECT DISTINCT(SoldAsVacant), COUNT(SoldAsVacant)
FROM NashvilleHousing
GROUP By SoldAsVacant
ORDER By 2

SELECT SoldAsVacant,
CASE 
    WHEN SoldAsVacant = 'Y' THEN 'Yes'
    WHEN SoldASVacant = 'N' THEN 'No'
    ELSE SoldAsVacant
END AS UpdatedValue
FROM NashvilleHousing

UPDATE NashvilleHousing
SET SoldAsVacant =  CASE WHEN SoldAsVacant = 'Y' THEN 'Yes'
    WHEN SoldASVacant = 'N' THEN 'No'
    ELSE SoldAsVacant
END

-- Removing Duplicates

WITH RowNumCTE AS(
SELECT *,
ROW_NUMBER() OVER (PARTITION By
            ParcelID,
            PropertyAddress,
            SaleDate,
            SalePrice,
            LegalReference,
            LandValue
            ORDER BY UniqueID
            ) rownum
FROM PortfolioProject..NashvilleHousing
)
DELETE
FROM RowNumCTE
WHERE rownum > 1

--Deleting unwanted columns

SELECT *
FROM PortfolioProject..NashvilleHousing

ALTER TABLE PortfolioProject..NashvilleHousing
DROP COLUMN PropertyAddress, SaleDate, OwnerAddress, TaxDistrict
