---
title: Приложение C. Типы данных FileX для ОСРВ Azure
description: Сведения о типах данных FileX для ОСРВ Azure.
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 26690df52917a4467c2ba2d3b59ee0e64277598b02471d70104ff1f584ad1543
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116783209"
---
# <a name="appendix-c---azure-rtos-filex-data-types"></a>Приложение C. Типы данных FileX для ОСРВ Azure

## <a name="fx_dir_entry"></a>FX_DIR_ENTRY

```c
typedef struct FX_DIR_ENTRY_STRUCT
{
    CHAR        *fx_dir_entry_name;
    CHAR        fx_dir_entry_short_name[FX_MAX_SHORT_NAME_LEN];
    UINT        fx_dir_entry_long_name_present;
    UINT        fx_dir_entry_long_name_shorted;
    UCHAR       fx_dir_entry_attributes;
    UCHAR       fx_dir_entry_reserved;
    UCHAR       fx_dir_entry_created_time_ms;
    UINT        fx_dir_entry_created_time;
    UINT        fx_dir_entry_created_date;
    UINT        fx_dir_entry_last_accessed_date;
    UINT        fx_dir_entry_time;
    UINT        fx_dir_entry_date;
    ULONG       fx_dir_entry_cluster;
    ULONG64     fx_dir_entry_file_size;
    ULONG64     fx_dir_entry_log_sector;
    ULONG       fx_dir_entry_byte_offset;
    ULONG       fx_dir_entry_number;
    ULONG       fx_dir_entry_last_search_cluster;
    ULONG       fx_dir_entry_last_search_relative_cluster;
    ULONG64     fx_dir_entry_last_search_log_sector;
    ULONG       fx_dir_entry_last_search_byte_offset;
    ULONG64     fx_dir_entry_next_log_sector;

    #ifdef FX_ENABLE_EXFAT
        CHAR        fx_dir_entry_dont_use_fat;
        UCHAR       fx_dir_entry_type;
        ULONG64     fx_dir_entry_available_file_size;
        ULONG       fx_dir_entry_secondary_count;
    #endif

} FX_DIR_ENTRY;
```

## <a name="fx_path"></a>FX_PATH

```c
typedef struct FX_PATH_STRUCT
{
    FX_DIR_ENTRY    fx_path_directory;
    CHAR            fx_path_string[FX_MAXIMUM_PATH];
    CHAR            fx_path_name_buffer[FX_MAX_LONG_NAME_LEN];
    ULONG           fx_path_current_entry;
} FX_PATH;
```

## <a name="fx_cached_sector"></a>FX_CACHED_SECTOR

```c
typedef FX_PATH FX_LOCAL_PATH;
typedef struct FX_CACHED_SECTOR_STRUCT
{
    UCHAR            *fx_cached_sector_memory_buffer;
    ULONG            fx_cached_sector;
    UCHAR            fx_cached_sector_buffer_dirty;
    UCHAR            fx_cached_sector_valid;
    UCHAR            fx_cached_sector_type; 
    UCHAR            fx_cached_sector_reserved; 
    struct           FX_CACHED_SECTOR_STRUCT
                         *fx_cached_sector_next_used; 
} FX_CACHED_SECTOR;
```

## <a name="fx_media"></a>FX_MEDIA

```c
typedef struct        FX_MEDIA_STRUCT
{
    ULONG        fx_media_id;
    CHAR        *fx_media_name;
    UCHAR       *fx_media_memory_buffer;
    ULONG        fx_media_memory_size;
    UINT        fx_media_sector_cache_hashed;
    ULONG        fx_media_sector_cache_size; 
    UCHAR       *fx_media_sector_cache_end; 
    struct        FX_CACHED_SECTOR_STRUCT
                    *fx_media_sector_cache_list_ptr;
    ULONG        fx_media_sector_cache_hashed_sector_valid;
    ULONG        fx_media_sector_cache_dirty_count;
    UINT        fx_media_bytes_per_sector;
    UINT        fx_media_sectors_per_track;
    UINT        fx_media_heads;
    ULONG64        fx_media_total_sectors;
    ULONG        fx_media_total_clusters;

    #ifdef FX_ENABLE_EXFAT
        ULONG        fx_media_exfat_volume_serial_number;
        UINT        fx_media_exfat_file_system_revision;
        UINT        fx_media_exfat_volume_flag;
        USHORT        fx_media_exfat_drive_select;
        USHORT        fx_media_exfat_percent_in_use;
        UINT        fx_media_exfat_bytes_per_sector_shift;
        UINT        fx_media_exfat_sector_per_clusters_shift;
        UCHAR        fx_media_exfat_bitmap_cache[512];
        ULONG        fx_media_exfat_bitmap_start_sector;
        ULONG        fx_media_exfat_bitmap_cache_size_in_sectors;
        ULONG        fx_media_exfat_bitmap_cache_start_cluster;
        ULONG        fx_media_exfat_bitmap_cache_end_cluster;
        UINT        fx_media_exfat_bitmap_clusters_per_sector_shift;
        UINT        fx_media_exfat_bitmap_cache_dirty;
    #endif

    UINT        fx_media_reserved_sectors;
    UINT        fx_media_root_sector_start; 
    UINT        fx_media_root_sectors;
    UINT        fx_media_data_sector_start;
    UINT        fx_media_sectors_per_cluster;
    UINT        fx_media_sectors_per_FAT;
    UINT        fx_media_number_of_FATs;
    UINT        fx_media_12_bit_FAT;
    UINT        fx_media_32_bit_FAT;
    ULONG        fx_media_FAT32_additional_info_sector;
    UINT        fx_media_FAT32_additional_info_last_available;

    #ifdef FX_DRIVER_USE_64BIT_LBA
        ULONG64        fx_media_hidden_sectors;
    #else
        ULONG        fx_media_hidden_sectors;
    #endif

    ULONG        fx_media_root_cluster_32;
    UINT        fx_media_root_directory_entries;
    ULONG        fx_media_available_clusters;
    ULONG        fx_media_cluster_search_start;
    VOID        *fx_media_driver_info;
    UINT        fx_media_driver_request;
    UINT        fx_media_driver_status;
    UCHAR       *fx_media_driver_buffer;

    #ifdef FX_DRIVER_USE_64BIT_LBA
        ULONG64        fx_media_driver_logical_sector;
    #else
        ULONG        fx_media_driver_logical_sector;
    #endif

    ULONG        fx_media_driver_sectors;
    ULONG        fx_media_driver_physical_sector;
    UINT        fx_media_driver_physical_track;
    UINT        fx_media_driver_physical_head;
    UINT        fx_media_driver_write_protect;
    UINT        fx_media_driver_free_sector_update;
    UINT        fx_media_driver_system_write;
    UINT        fx_media_driver_data_sector_read;
    UINT        fx_media_driver_sector_type;

    VOID        (*fx_media_driver_entry)(struct        fx_MEDIA_STRUCT *); 
    VOID        (*fx_media_open_notify)(struct        fx_MEDIA_STRUCT *); 
    VOID        (*fx_media_close_notify)(struct        fx_MEDIA_STRUCT *); 
    struct        FX_FILE_STRUCT
                    *fx_media_opened_file_list; 
    ULONG        fx_media_opened_file_count; 
    struct        FX_MEDIA_STRUCT
                    *fx_media_opened_next,
                    *fx_media_opened_previous;

    #ifndef FX_MEDIA_STATISTICS_DISABLE
        ULONG        fx_media_directory_attributes_reads;
        ULONG        fx_media_directory_attributes_sets;
        ULONG        fx_media_directory_creates;
        ULONG        fx_media_directory_default_gets;
        ULONG        fx_media_directory_default_sets;
        ULONG        fx_media_directory_deletes;
        ULONG        fx_media_directory_first_entry_finds;
        ULONG        fx_media_directory_first_full_entry_finds;
        ULONG        fx_media_directory_information_gets;
        ULONG        fx_media_directory_local_path_clears;
        ULONG        fx_media_directory_local_path_gets;
        ULONG        fx_media_directory_local_path_restores;
        ULONG        fx_media_directory_local_path_sets;
        ULONG        fx_media_directory_name_tests;
        ULONG        fx_media_directory_next_entry_finds;
        ULONG        fx_media_directory_next_full_entry_finds;
        ULONG        fx_media_directory_renames;
        ULONG        fx_media_file_allocates;
        ULONG        fx_media_file_attributes_reads;
        ULONG        fx_media_file_attributes_sets;
        ULONG        fx_media_file_best_effort_allocates;
        ULONG        fx_media_file_closes;
        ULONG        fx_media_file_creates;
        ULONG        fx_media_file_deletes;
        ULONG        fx_media_file_opens;
        ULONG        fx_media_file_reads;
        ULONG        fx_media_file_relative_seeks;
        ULONG        fx_media_file_renames;
        ULONG        fx_media_file_seeks;
        ULONG        fx_media_file_truncates;
        ULONG        fx_media_file_truncate_releases;
        ULONG        fx_media_file_writes;
        ULONG        fx_media_aborts;
        ULONG        fx_media_flushes;
        ULONG        fx_media_reads;
        ULONG        fx_media_writes;
        ULONG        fx_media_directory_entry_reads;
        ULONG        fx_media_directory_entry_writes;
        ULONG        fx_media_directory_searches;
        ULONG        fx_media_directory_free_searches;
        ULONG        fx_media_fat_entry_reads;
        ULONG        fx_media_fat_entry_writes;
        ULONG        fx_media_fat_entry_cache_read_hits;
        ULONG        fx_media_fat_entry_cache_read_misses;
        ULONG        fx_media_fat_entry_cache_write_hits;
        ULONG        fx_media_fat_entry_cache_write_misses;
        ULONG        fx_media_fat_cache_flushes;
        ULONG        fx_media_fat_sector_reads;
        ULONG        fx_media_fat_sector_writes;
        ULONG        fx_media_logical_sector_reads;
        ULONG        fx_media_logical_sector_writes;
        ULONG        fx_media_logical_sector_cache_read_hits;
        ULONG        fx_media_logical_sector_cache_read_misses;
        ULONG        fx_media_driver_read_requests;
        ULONG        fx_media_driver_write_requests;
        ULONG        fx_media_driver_boot_read_requests;
        ULONG        fx_media_driver_boot_write_requests;
        ULONG        fx_media_driver_release_sectors_requests;
        ULONG        fx_media_driver_flush_requests;
    #endif

    #ifndef FX_MEDIA_DISABLE_SEARCH_CACHE
        ULONG        fx_media_directory_search_cache_hits;
    #endif

    #ifndef FX_SINGLE_THREAD
        TX_MUTEX        fx_media_protect;
    #endif

    #ifndef FX_MEDIA_DISABLE_SEARCH_CACHE
        UINT        fx_media_last_found_directory_valid;
        FX_DIR_ENTRY        fx_media_last_found_directory;
        FX_DIR_ENTRY        fx_media_last_found_entry;
        CHAR        fx_media_last_found_file_name[FX_MAX_LONG_NAME_LEN];
        CHAR        fx_media_last_found_name[FX_MAX_LAST_NAME_LEN];
    #endif

    FX_PATH        fx_media_default_path;        
    FX_FAT_CACHE_ENTRY    fx_media_fat_cache[FX_MAX_FAT_CACHE];
    UCHAR        fx_media_fat_secondary_update_map[FX_FAT_MAP_SIZE];
    ULONG        fx_media_reserved_for_user;
    CHAR        fx_media_name_buffer[4*FX_MAX_LONG_NAME_LEN];

    #ifdef FX_RENAME_PATH_INHERIT
        CHAR        fx_media_rename_buffer[FX_MAXIMUM_PATH]; 
        struct        FX_CACHED_SECTOR_STRUCT
        fx_media_sector_cache[FX_MAX_SECTOR_CACHE];
        ULONG        fx_media_sector_cache_hash_mask;
        ULONG        fx_media_disable_burst_cache;
    #ifdef FX_ENABLE_FAULT_TOLERANT
        UCHAR        fx_media_fault_tolerant_enabled;
        UCHAR        fx_media_fault_tolerant_state;
        USHORT        fx_media_fault_tolerant_transaction_count;
        ULONG        fx_media_fault_tolerant_start_cluster;
        ULONG        fx_media_fault_tolerant_clusters;
        ULONG        fx_media_fault_tolerant_total_logs;
        UCHAR       *fx_media_fault_tolerant_memory_buffer;
        ULONG        fx_media_fault_tolerant_memory_buffer_size;
        ULONG        fx_media_fault_tolerant_file_size; 
        ULONG        fx_media_fault_tolerant_cached_FAT_sector;
        fx_media_fault_tolerant_cache[FX_FAULT_TOLERANT_CACHE_SIZE >> 2]; 
        ULONG        fx_media_fault_tolerant_cached_FAT_sector;
    #endif

    ULONG        fx_media_fat_reserved;
    ULONG        fx_media_fat_last;
    UCHAR        fx_media_FAT_type;
} FX_MEDIA;
```

## <a name="fx_file"></a>FX_FILE

```c

typedef struct FX_FILE_STRUCT
{
    ULONG         fx_file_id;
    CHAR *        fx_file_name;
    ULONG         fx_file_open_mode;
    UCHAR         fx_file_modified;
    ULONG         fx_file_total_clusters;
    ULONG         fx_file_first_physical_cluster;
    ULONG         fx_file_consecutive_cluster;
    ULONG         fx_file_last_physical_cluster;
    ULONG         fx_file_current_physical_cluster;
    ULONG64     fx_file_current_logical_sector;
    ULONG         fx_file_current_logical_offset;
    ULONG         fx_file_current_relative_cluster;
    ULONG         fx_file_current_relative_sector;
    ULONG64     fx_file_current_file_offset;
    ULONG64     fx_file_current_file_size;
    ULONG64     fx_file_current_available_size;

    #ifdef FX_ENABLE_FAULT_TOLERANT
        ULONG64     fx_file_maximum_size_used;
    #endif /*FX_ENABLE_FAULT_TOLERANT */

    FX_MEDIA    *fx_file_media_ptr; 
    struct         FX_FILE_STRUCT
                    *fx_file_opened_next,
                    *fx_file_opened_previous;
    FX_DIR_ENTRY fx_file_dir_entry;
    CHAR        fx_file_name_buffer[FX_MAX_LONG_NAME_LEN];
    ULONG         fx_file_disable_burst_cache;
    VOID        (*fx_file_write_notify)(struct FX_FILE_STRUCT *); 
} FX_FILE;

```