# zizi-repo
like a moon
import pandas as pd
import matplotlib.pyplot as plt

def process_csv(file_path, column_name):
    try:
        # Read CSV file
        df = pd.read_csv(file_path)

        if column_name not in df.columns:
            print(f"❌ Column '{column_name}' does not exist in the file.")
            return

        # Calculate basic statistic
        mean_value = df[column_name].mean()
        median_value = df[column_name].median()
        max_value = df[column_name].max()
        min_value = df[column_name].min()
        std_dev = df[column_name].std()

        # Print results
        print(f"📊 Processing column: {column_name}")
        print(f"  Mean: {mean_value:.2f}")
        print(f"  Median: {median_value:.2f}")
        print(f"  Max: {max_value}")
        print(f"  Min: {min_value}")
        print(f"  Standard Deviation: {std_dev:.2f}")

        # Plot histogram
        plt.figure(figsize=(10, 4))
        plt.subplot(1, 2, 1)
        plt.hist(df[column_name], bins=20, color='skyblue', edgecolor='black')
        plt.title(f"Histogram of {column_name}")
        plt.xlabel(column_name)
        plt.ylabel("Frequency")

        # Plot boxplot
        plt.subplot(1, 2, 2)
        plt.boxplot(df[column_name], vert=False)
        plt.title(f"Boxplot of {column_name}")

        plt.tight_layout()
        plt.show()

    except FileNotFoundError:
        print("❌ File not found. Please check the file path.")
    except Exception as e:
        print(f"⚠️ Error: {e}")

# Example usage:
# process_csv("data.csv", "price")
